# SpringBoot与Tomcat的启动（内嵌Tomcat）



环境：
 SpringBoot 2.0.1

使用SpringBoot开发时，可以通过Maven将工程打成jar包，jar包内嵌Tomcat，这种方式SpringBoot工程将在启动的时候，带动Tomcat的启动，下面分析SpringBoot如何带动Tomcat启动

一个简单的SpringBoot工程启动类

```
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

跟进`SpringApplication#run`方法

```
public class SpringApplication {

    public static ConfigurableApplicationContext run(Class<?> primarySource, 
            String... args) {
        return run(new Class<?>[] { primarySource }, args);
    }

    public static ConfigurableApplicationContext run(Class<?>[] primarySources,
            String[] args) {
        return new SpringApplication(primarySources).run(args);
    }

    // 保存当前Web工程类型
    private WebApplicationType webApplicationType;
    // 保存当前工程的主启动类
    private Class<?> mainApplicationClass;

    // 重要，本方法内部推断了当前Web应用类型以及启动类
    public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
        this.resourceLoader = resourceLoader;
        this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
        this.webApplicationType = deduceWebApplicationType();
        setInitializers((Collection) getSpringFactoriesInstances(
                ApplicationContextInitializer.class));
        setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
        this.mainApplicationClass = deduceMainApplicationClass();
    }

    // 三个用于判断当前Web工程类型的类
    private static final String[] WEB_ENVIRONMENT_CLASSES = { "javax.servlet.Servlet",
            "org.springframework.web.context.ConfigurableWebApplicationContext" };
    private static final String REACTIVE_WEB_ENVIRONMENT_CLASS = "org.springframework."
            + "web.reactive.DispatcherHandler";
    private static final String MVC_WEB_ENVIRONMENT_CLASS = "org.springframework."
            + "web.servlet.DispatcherServlet";

    // 通过上面的几个类是否存在，推断当前Web工程类型
    private WebApplicationType deduceWebApplicationType() {
        if (ClassUtils.isPresent(REACTIVE_WEB_ENVIRONMENT_CLASS, null)
                && !ClassUtils.isPresent(MVC_WEB_ENVIRONMENT_CLASS, null)) {
            return WebApplicationType.REACTIVE;
        }
        for (String className : WEB_ENVIRONMENT_CLASSES) {
            if (!ClassUtils.isPresent(className, null)) {
                return WebApplicationType.NONE;
            }
        }
        return WebApplicationType.SERVLET;
    }

    // 推断当前工程主启动类
    private Class<?> deduceMainApplicationClass() {
        try {
            StackTraceElement[] stackTrace = new RuntimeException().getStackTrace();
            for (StackTraceElement stackTraceElement : stackTrace) {
                if ("main".equals(stackTraceElement.getMethodName())) {
                    return Class.forName(stackTraceElement.getClassName());
                }
            }
        } catch (ClassNotFoundException ex) {
            // Swallow and continue
        }
        return null;
    }

    // 开始启动
    public ConfigurableApplicationContext run(String... args) {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        ConfigurableApplicationContext context = null;
        Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
        configureHeadlessProperty();
        SpringApplicationRunListeners listeners = getRunListeners(args);
        listeners.starting();
        try {
            // ...
            // 创建ApplicationContext容器
            context = createApplicationContext();
            // ....
            // 刷新容器
            refreshContext(context);
            // ....
        } catch (Throwable ex) {
            // ...
        }
        // ...
        return context;
    }

    private void refreshContext(ConfigurableApplicationContext context) {
        refresh(context);
        if (this.registerShutdownHook) {
            try {
                context.registerShutdownHook();
            } catch (AccessControlException ex) {
                // Not allowed in some environments.
            }
        }
    }

    protected void refresh(ApplicationContext applicationContext) {
        // ...
        // 执行容器刷新
        ((AbstractApplicationContext) applicationContext).refresh();
    }
}
```

如果对Spring的启动流程有了解的话，应该知道Spring启动过程中，最重要的就是`AbstractApplicationContext#refresh()`过程，在这个方法中Spring执行了BeanFactory的初始化，Bean的实例化、属性填充、初始化操作等等重要的操作，该方法主要逻辑如下：

```
public abstract class AbstractApplicationContext 
            extends DefaultResourceLoader
            implements ConfigurableApplicationContext {

    @Override
    public void refresh() throws BeansException, IllegalStateException {
        synchronized (this.startupShutdownMonitor) {
            // Prepare this context for refreshing.
            prepareRefresh();
            // Tell the subclass to refresh the internal bean factory.
            ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
            // Prepare the bean factory for use in this context.
            prepareBeanFactory(beanFactory);
            try {
                // Allows post-processing of the bean factory in context subclasses.
                postProcessBeanFactory(beanFactory);
                // Invoke factory processors registered as beans in the context.
                invokeBeanFactoryPostProcessors(beanFactory);
                // Register bean processors that intercept bean creation.
                registerBeanPostProcessors(beanFactory);
                // Initialize message source for this context.
                initMessageSource();
                // Initialize event multicaster for this context.
                initApplicationEventMulticaster();
                // Initialize other special beans in specific context subclasses.
                onRefresh();
                // Check for listener beans and register them.
                registerListeners();
                // Instantiate all remaining (non-lazy-init) singletons.
                finishBeanFactoryInitialization(beanFactory);
                // Last step: publish corresponding event.
                finishRefresh();
            } catch (BeansException ex) {
                // 省略日志输出...
                // Destroy already created singletons to avoid dangling resources.
                destroyBeans();
                // Reset 'active' flag.
                cancelRefresh(ex);
                // Propagate exception to caller.
                throw ex;
            } finally {
                // Reset common introspection caches in Spring's core, since we
                // might not ever need metadata for singleton beans anymore...
                resetCommonCaches();
            }
        }
    }
}
```

仔细观察这个方法，可以发现其中调用了几个空方法，这里应用了`模板模式`，在`refresh`方法中定义了主干执行流程，并留有空的方法给子类做个性化定制

```
public abstract class AbstractApplicationContext 
            extends DefaultResourceLoader
            implements ConfigurableApplicationContext {
    
    protected void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) { 
        // 空实现
    }

    protected void onRefresh() throws BeansException {
        // 空实现
    }
}
```

现在开始，通过debug来看看SpringBoot如何启动Tomcat容器

前面构造`SpringApplicaton`时，已经推断出当前Web工程类型，当开始执行`#run`方法时，会根据不同类型的Web项目创建不同类型的ApplicationContext

```
public class SpringApplication {

    public ConfigurableApplicationContext run(String... args) {
        // ... 各种省略
        // 创建一个SpringBoot的应用上下文
        context = createApplicationContext();
       // 刷新容器
        refreshContext(context);
        return context;
    }

    public static final String DEFAULT_WEB_CONTEXT_CLASS = "org.springframework.boot."
            + "web.servlet.context.AnnotationConfigServletWebServerApplicationContext";
    public static final String DEFAULT_REACTIVE_WEB_CONTEXT_CLASS = "org.springframework."
            + "boot.web.reactive.context.AnnotationConfigReactiveWebServerApplicationContext";
    public static final String DEFAULT_CONTEXT_CLASS = "org.springframework.context."
            + "annotation.AnnotationConfigApplicationContext";

    // 根据deduceWebApplicationType推断出来的webApplicationType
    // 选择不同类型的ApplicationContext创建
    protected ConfigurableApplicationContext createApplicationContext() {
        Class<?> contextClass = this.applicationContextClass;
        if (contextClass == null) {
            try {
                switch (this.webApplicationType) {
                    case SERVLET:
                        // 本文将会根据此行代码创建容器对象
                        // 类型：AnnotationConfigServletWebServerApplicationContext
                        // 继承自：ServletWebServerApplicationContext
                        contextClass = Class.forName(DEFAULT_WEB_CONTEXT_CLASS);
                        break;
                    case REACTIVE:
                        contextClass = Class.forName(DEFAULT_REACTIVE_WEB_CONTEXT_CLASS);
                        break;
                    default:
                        contextClass = Class.forName(DEFAULT_CONTEXT_CLASS);
                }
            } catch (ClassNotFoundException ex) {
                // ... 省略异常处理
            }
        }
        return (ConfigurableApplicationContext) BeanUtils.instantiateClass(contextClass);
    }

    protected void refresh(ApplicationContext applicationContext) {
        ((AbstractApplicationContext) applicationContext).refresh();
    }
}
```

创建好`ApplicationContext`之后，看到`refreshContext(context)`，联系本文开头Spring启动的`AbstractApplicationContext#refresh`方法，该方法其实就是触发了后者的执行

执行`refreshContext`方法，来到前面根据`webApplicationType`创建的容器类`ServletWebServerApplicationContext`

```
public class ServletWebServerApplicationContext 
            extends GenericWebApplicationContext
            implements ConfigurableWebServerApplicationContext {

    // 这里其实就是调用了AbstractApplicationContext#refresh
    @Override
    public final void refresh() 
            throws BeansException, IllegalStateException {
        try {
            super.refresh();
        } catch (RuntimeException ex) {
            stopAndReleaseWebServer();
            throw ex;
        }
    }

    // 重点是这里，它重写了AbstractApplicationContext的onRefresh
    // 并且在这类创建了一个web服务器
    @Override
    protected void onRefresh() {
        super.onRefresh();
        try {
            createWebServer();
        } catch (Throwable ex) {
        }
    }

    private void createWebServer() {
        WebServer webServer = this.webServer;
        ServletContext servletContext = getServletContext();
        if (webServer == null && servletContext == null) {
            // 通过Spring.factories配置自动装配类
            // ServletWebServerFactoryAutoConfiguration
            // 该类通过Import引入ServletWebServerFactoryConfiguration#EmbeddedTomcat
            ServletWebServerFactory factory = getWebServerFactory();
            this.webServer = factory.getWebServer(getSelfInitializer());
        } else if (servletContext != null) {
            try {
                getSelfInitializer().onStartup(servletContext);
            } catch (ServletException ex) {
            }
        }
        initPropertySources();
    }
}
```

进入到`TomcatServletWebServerFactory`，可以看到如下启动内置Tomcat的过程

```
public class TomcatServletWebServerFactory 
            extends AbstractServletWebServerFactory
            implements ConfigurableTomcatWebServerFactory, ResourceLoaderAware {

    @Override
    public WebServer getWebServer(ServletContextInitializer... initializers) {
        Tomcat tomcat = new Tomcat();
        File baseDir = (this.baseDirectory != null ? this.baseDirectory
                : createTempDir("tomcat"));
        tomcat.setBaseDir(baseDir.getAbsolutePath());
        Connector connector = new Connector(this.protocol);
        tomcat.getService().addConnector(connector);
        customizeConnector(connector);
        tomcat.setConnector(connector);
        tomcat.getHost().setAutoDeploy(false);
        configureEngine(tomcat.getEngine());
        for (Connector additionalConnector : this.additionalTomcatConnectors) {
            tomcat.getService().addConnector(additionalConnector);
        }
        prepareContext(tomcat.getHost(), initializers);
        return getTomcatWebServer(tomcat);
    }

    protected TomcatWebServer getTomcatWebServer(Tomcat tomcat) {
        // 启动Tomcat
        return new TomcatWebServer(tomcat, getPort() >= 0);
    }
}
```

```
public class TomcatWebServer implements WebServer {

    public TomcatWebServer(Tomcat tomcat, boolean autoStart) {
        Assert.notNull(tomcat, "Tomcat Server must not be null");
        this.tomcat = tomcat;
        this.autoStart = autoStart;
        initialize();
    }

    private void initialize() throws WebServerException {
        synchronized (this.monitor) {
            try {
                addInstanceIdToEngineName();
                Context context = findContext();
                context.addLifecycleListener((event) -> {
                    if (context.equals(event.getSource())
                            && Lifecycle.START_EVENT.equals(event.getType())) {
                        removeServiceConnectors();
                    }
                });
                // Start the server to trigger initialization listeners
                this.tomcat.start();   // 启动Tomcat
                // We can re-throw failure exception directly in the main thread
                rethrowDeferredStartupExceptions();
                try {
                    ContextBindings.bindClassLoader(context, context.getNamingToken(),
                            getClass().getClassLoader());
                } catch (NamingException ex) {
                }

                // Unlike Jetty, all Tomcat threads are daemon threads. We create a
                // blocking non-daemon to stop immediate shutdown
                // 阻塞当前Tomcat线程，否则Tomcat就直接退出了 
                startDaemonAwaitThread();
            } catch (Exception ex) {
                stopSilently();
                throw new WebServerException("Unable to start embedded Tomcat", ex);
            }
        }
    }
}
```

至此，Tomcat继承Spring的`AbstractApplicationContext`类，覆盖它的模板方法`onRefresh`，SpringBoot在自身启动的过程中，启动了内置的Tomcat服务器

 

 

 

 

 