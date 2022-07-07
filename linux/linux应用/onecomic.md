# 一本漫画

**尊重版权，请支持正版**

**该项目仅供技术研究使用，请勿用于非法用途，否则后果自负**

**通过本工具下载或生成的资源禁止传播分享！禁止利用该项目从事营利性活动！**

项目地址： <https://github.com/hardwarecode/onecomic>

## 安装/升级步骤

自己找安装Python的教程（Python版本大于等于3.6）

安装nodejs环境，自己找教程安装

```
# 检查python版本
python --version
# 检查pip版本
pip --version
```

```
# 在线安装/升级（最新版本）
pip install -U onecomic

# 查看帮助
onecomic --help
```

## 常规使用

**Windows下，以下所有示例命令需要添加python -m前缀**

如: `python -m onecomic --url "http://ac.qq.com/Comic/ComicInfo/id/505430"`

```
# 注意参数里的 - 和 -- 的区别
# 从章节列表页面的URL 下载漫画的最新一集
onecomic --url "http://ac.qq.com/Comic/ComicInfo/id/505430"

# 下载漫画 id=505430 最新一集 注意不同站点的漫画id区别
onecomic -s qq -id=505430

# 下载所有章节
onecomic -s qq -id 505430 --all

# 下载第800集
onecomic -s qq -id 505430 -c 800

# 下载倒数第二集
onecomic -s qq -id 505430 -c -2

# 下载1到5集,7集，9到10集
onecomic -s qq -id 505430 -c 1-5,7,9-10

# 拼接成长图
onecomic -s qq -id 505430 --single-image --quality 95 --max-height 20000

# 压缩成zip文件
onecomic -s qq -id=505430 --zip

# 设置代理
onecomic -s qq -id 505430 --proxy "socks5://127.0.0.1:1080"

# 自定义保存目录
onecomic -s qq -id=505430 --output MyComicBook

# 将多话合并到单个文件夹和zip文件
onecomic -s manhuagui -id 1128 -c 320-322 --merge --merge-zip

# 下载单行本
onecomic -s manhuagui -id 1128 --ext-name 单行本 -c -1

# 跟据名字搜索comicid
onecomic -s qq --name 海贼

# 生成pdf文件
# 注意: 生成pdf文件需要额外安装依赖，需要先执行 pip install img2pdf 或 pip install reportlab
onecomic -s qq -id 505430 --pdf

# 推送到邮箱
# 注意: 发送到邮箱需预先配置好信息（详情请看配置文件部分）
onecomic -s qq -id 505430 --pdf --mail --config config.ini
```

从其它站点下载，注意不同站点的comicid区别

```
# 从哔哩哔哩漫画下载
onecomic -s bilibili -id mc24742 -c 1

# 从有妖气漫画下载
onecomic -s u17 -id 195 -c 1

# 从章节列表页面的URL下载
onecomic --url "https://manga.bilibili.com/detail/mc28603" -c 1
```

## 关于登录

登录后可下载已购买的付费资源

1. [安装EditThisCookie插件](https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg)
2. 在浏览器上登录某个站点，然后通过插件导出某个站点的cookies，并保存到本地文件 如`qq.json`

```
onecomic -s qq -id=505430 -c -1 --cookies-path="qq.json"
```

## 高级批量下载

```
# 通过指定的URL文件列表批量下载
onecomic --url-file test/test-url-file.txt
```

文件示例`test/test-url-file.txt`

```
# 海贼王
http://ac.qq.com/Comic/ComicInfo/id/505430
# 雏蜂
https://www.u17.com/comic/195.html
```

------

```
# 有些站点不一定支持，其它的通用参数也适用，可自行组合
# 下载最近更新页面的1到10页 所有漫画的最新一集
onecomic -s nvshens --latest-all --latest-page 1-10

# 展示支持的标签
onecomic -s nvshens --show-tags

# 下载标签搜索结果页面的1到10页 所有漫画的全集
onecomic -s nvshens --tag-all --tag 女神 --tag-page 1-10 --all

# 下载搜索结果的所有漫画的全集
onecomic -s nhentai --search-all --search-name 汉化 --search-page 1 --all
```

## 其它说明

### cookies使用说明

- 下载付费内容需要cookies，Toomics、qootoon站点下载R18内容也需要cookies
- 最好保证脚本与在浏览器导出的cookies在同一个网络环境（如果浏览器使用代理，脚本也要使用同样的代理环境）
- 若使用了cookies还是下载不了，需检查账号在站点浏览是否正常，若正常浏览则重新导出一份新的cookies文件再做尝试
- 若还是下载不了，请加群反馈。付费内容的下载问题还请提供cookies或账号（私聊群主），不提供大概率会被无视

### 关于cocomanhua的下载

1. 安装nodejs环境
2. 安装crypto-js依赖，命令：`npm install crypto-js`，默认在当前目录下生成`node_modules`目录
3. 下载 `onecomic -s cocomanhua -id 12187` 或者 `onecomic -s cocomanhua --node-modules ./node_modules`







# 配置文件

根据示例创建配置文件

## 配置文件示例

```
[mail]

# SMTP主机 发送账户是163邮箱则设置为 smtp.163.com
smtp_server=smtp.qq.com

# SMTP服务端口（SSL）
smtp_port=465

# 发送者账户
sender=1803680479.qq.com

# 登录失败可能需要使用授权码登录 https://service.mail.qq.com/cgi-bin/help?subtype=1&&id=28&&no=1001256
# 发送者账户密码或者授权码
sender_passwd=22887321deng

# 邮件接收者列表，多个则以半角逗号隔开。如 xxx@qq.com,yyy@qq.com
# receivers=11111@qq.com,22222@qq.com


[crawler]
# 默认的下载目录
# download_dir=/home/kimihiro/comics/MyComicBook

# 配置指定站点代理地址
# proxy_18comic=socks5://127.0.0.1:1080
# proxy_manhuagui=socks5://127.0.0.1:1080
# proxy_nhentai=socks5://127.0.0.1:1080
# proxy_wnacg=socks5://127.0.0.1:1080
# proxy_acg456=socks5://127.0.0.1:1080
# proxy_mh1234=socks5://127.0.0.1:1080
# proxy_177pic=socks5://127.0.0.1:1080
# proxy_18hmmcg=socks5://127.0.0.1:1080
# proxy_xiuren=socks5://127.0.0.1:1080
# proxy_twhentai=socks5://127.0.0.1:1080
# proxy_copymanga=socks5://127.0.0.1:1080
# proxy_toomics=socks5://127.0.0.1:1080
# proxy_webtoons=socks5://127.0.0.1:1080


# 若站点域名变更，页面样式不变，可通过参数修改域名
# site_index_gufengmh=https://www.gufengmh9.com/
# site_index_mh160=https://mh160.cc/


# webdriver 配置
# driver_type=Chrome
# driver_path=/home/xxx/chromedriver_win32/chromedriver.exe


# node模块位置
# node_modules=/home/xxx/js/node_modules

# cookies存放目录（自动读取该目录下的cookeis文件），文件名命名规范: 如 toomics.json qq.json {site}.json
# cookies_dir=/home/xxx/cookies

# 长图质量 最大100
# quality=95
# 长图最大高度 最大65500
# max_height=20000

# 图片下载超时时间 单位秒
image_timeout=30

# 站点访问超时时间 单位秒
crawler_timeout=45

# 每个章节下载时间间隔 单位秒
crawler_delay=1

# User-Agent 配置
# user_agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.82 Safari/537.36

# 将下载的webp格式的图片自动转换成jpg
transfer_webp=1
```

将上述配置保存为`config.ini`

如`onecomic -s u17 -id 195 -c 1`会默认读取当前目录下的`config.ini`配置文件

或者通过环境变量配置默认的配置文件

```
# 将以下命令添加到 ~/.bashrc 或 ~/.zshrc 文件末尾
export ONECOMIC_CONFIG_FILE="/home/xxx/MyConfig/config.ini"
```

也可以通过参数指定配置文件`onecomic -s u17 -id 195 -c 1 --config config.ini`



## 更新

```
# 只更新onecomic（修复bug、添加更多的源等）
pip install -U onecomic

# 更新项目。注意`api/config.py.example`有无新增的配置，如果有的话需要重新配置`api/config.py`
git pull
pip install -U onecomic
pip install -r requirements.txt
```





# 站点收录情况

## 漫画站点

| 站点               | site       | 站点主页                         | 最近更新 | 关键词搜索 | 标签搜索 |
| ------------------ | ---------- | -------------------------------- | -------- | ---------- | -------- |
| 拷贝漫画           | copymanga  | <https://copymanga.com/>         | ✓        | ✓          | ✓        |
| 二次元动漫         | 2animx     | <https://www.2animx.com/>        | ✓        | ✓          | ✓        |
| 古风漫画网         | gufengmh   | <https://www.gufengmh9.com/>     | ✓        | -          | ✓        |
| 36漫画网           | 36mh       | <https://www.36mh.com/>          | ✓        | -          | ✓        |
| 土豪漫画网         | tuhao456   | <https://www.tuhao456.com/>      | ✓        | ✓          | ✓        |
| 漫画DB             | manhuadb   | <https://www.manhuadb.com/>      | ✓        | ✓          | ✓        |
| DM5                | dm5        | <https://www.dm5.com/>           | ✓        | ✓          | ✓        |
| bilibili漫画       | bilibili   | <https://manga.bilibili.com/>    | ✓        | ✓          | ✓        |
| 腾讯漫画           | qq         | <https://ac.qq.com/>             | ✓        | ✓          | ✓        |
| 有妖气漫画         | u17        | <https://www.u17.com/>           | ✓        | ✓          | ✓        |
| 动漫之家           | dmzj       | <https://www.dmzj.com/>          | ✓        | ✓          | ✓        |
| 快看漫画           | kuaikan    | <https://www.kuaikanmanhua.com/> | ✓        | ✓          | ✓        |
| 漫画柜             | manhuagui  | <https://www.manhuagui.com/>     | ✓        | ✓          | ✓        |
| 漫画柜             | mhgui      | <https://www.mhgui.com/>         | ✓        | ✓          | ✓        |
| 漫画1234           | mh1234     | <https://www.mh1234.com/>        | ✓        | ✓          | ✓        |
| 新新漫画           | 77mh       | <https://www.77mh.cc/>           | ✓        | ✓          | ✓        |
| 漫画台             | manhuatai  | <https://www.manhuatai.com/>     | ✓        | -          | -        |
| ACG肆伍陆          | acg456     | <http://www.acg456.com/>         | ✓        | -          | ✓        |
| COCO漫画           | cocomanhua | <https://www.cocomanhua.com/>    | ✓        | ✓          | ✓        |
| WEBTOON            | webtoons   | <https://www.webtoons.com/>      | ✓        | -          | -        |
| qootoon            | qootoon    | <https://www.qootoon.net/>       | ✓        | -          | -        |
| 扑飞漫画           | pufei8     | <http://www.pufei8.com/>         | -        | -          | ✓        |
| 歪漫屋             | yymh889    | <http://yymh889.com/>            | ✓        | ✓          | -        |
| 爱飞漫画           | 2feimh     | <https://www.2feimh.com/>        | -        | -          | -        |
| 优酷漫画           | ykmh       | <https://www.ykmh.com/>          | ✓        | ✓          | -        |
| 来漫画             | laimanhua  | <https://www.laimanhua.net/>     | ✓        | ✓          | ✓        |
| 漫画160            | mh160      | <https://www.mh160.cc/>          | ✓        | ✓          | ✓        |
| 奇漫屋             | qiman6     | <http://www.qiman6.com/>         | ✓        | ✓          | ✓        |
| 奇妙漫画           | qimiaomh   | <https://www.qimiaomh.com/>      | ✓        | ✓          | ✓        |
| 6漫画              | sixmh6     | <http://www.sixmh6.com/>         | ✓        | ✓          | ✓        |
| 波动boodo          | boodo      | <https://boodo.qq.com/>          | ✓        | ✓          | ✓        |
| 3250漫画（已失效） | 3250mh     | <https://www.3250mh.com/>        | ✓        | ✓          | ✓        |
| 漫番漫画           | myfcomic   | <http://www.myfcomic.com/>       | ✓        | ✓          | ✓        |
| 包子漫画           | baozimh    | <https://www.baozimh.com/>       | ✓        | ✓          | ✓        |
| BoyLove            | boylove    | <https://boylove.cc/>            | ✓        | ✓          | ✓        |
| 酷爱漫画           | kuimh      | <https://www.kuimh.com/>         | ✓        | ✓          | ✓        |

## R18

| 站点      | site    | 站点主页                   | 最近更新 | 关键词搜索 | 标签搜索 |
| --------- | ------- | -------------------------- | -------- | ---------- | -------- |
| 18H漫画区 | 18hmmcg | <http://18h.mm-cg.com/>    | ✓        | ✓          | ✓        |
| 177漫畫   | 177pic  | <http://177pic.info/>      | ✓        | ✓          | ✓        |
| 绅士漫画  | wnacg   | <http://www.wnacg.org/>    | ✓        | ✓          | ✓        |
| 禁漫天堂  | 18comic | <https://18comic.org/>     | ✓        | ✓          | ✓        |
| NHentai   | nhentai | <https://nhentai.net/>     | ✓        | ✓          | ✓        |
| 禁漫之家  | jmzj    | <http://jmzj.xyz/>         | ✓        | ✓          | ✓        |
| 污污漫画  | 55comic | <https://www.55comic.com/> | ✓        | ✓          | ✓        |

## 图集

| 站点   | site   | 站点主页                 | 最近更新 | 关键词搜索 | 标签搜索 |
| ------ | ------ | ------------------------ | -------- | ---------- | -------- |
| 秀人网 | xiuren | <http://www.xiuren.org/> | ✓        | -          | ✓        |
| MMKK   | mmkk   | <https://www.mmkk.me/>   | ✓        | -          | ✓        |

## 其它站点（未收录）

| 站点     | 说明                         | 站点主页                |
| -------- | ---------------------------- | ----------------------- |
| e-hentai | 配合 Ehviewer 食用           | <https://e-hentai.org/> |
| exhenta  | 配合 Ehviewer 食用           | <https://exhentai.org/> |
| Nsfwpicx | NSFW图集（看看就好，别爬了） | <http://picxxxxx.top/>  |
| Toomics  | 之前适配过，现在不能用了     | <https://toomics.com/>  |

