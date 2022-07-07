# ubuntu安装vue

### 安装node.js

```
apt install nodejs
```

查看nodejs版本

```
node -V
```



### 安装npm

```
apt install npm
```

查看npm版本

```
npm -V
```



### 查看registry镜像地址

```
npm get registry
```

### 设置淘宝镜像

```
npm config set registry http://registry.npm.taobao.org 
```

### 安装cnpm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 安装vue脚手架

```
cnpm install -g @vue/cli
```

### 使用 cnpm 安装 webpack

```
cnpm install webpack -g
```

