# hexoBlog

##几个常用的hexo命令
hexo new "postName"             // 新建文章
hexo new page "pageName"        // 新建页面
hexo clean                      // 清理目录
hexo generate                   // 生成静态页面至public目录
hexo server                     //开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy                     // 将.deploy目录部署到GitHub

### 以上命令对应的简写
hexo n
hexo clean
hexo g
hexo s
hexo d

### 当然，每次一个命令也是比较烦的，尤其是调试效果的时候，所以可以这样
hexo s -g                       // 先生成再启动
hexo s -g -p 5000               // 先生成再启动，端口使用5000

这里调试的时候直接刷新页面就可以看到更新，之前不知道净关闭，重启了。也是醉了！
如果写个脚本来执行也可以。

## hexo
npm install hexo -g #安装  
npm update hexo -g #升级  
hexo init #初始化

## 简写
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo p == hexo publish
hexo g == hexo generate#生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy#部署

## 服务器
hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP

hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署

## 监视文件变动
hexo generate #使用 Hexo 生成静态文件快速而且简单
hexo generate --watch #监视文件变动

## 完成后部署
两个命令的作用是相同的
hexo generate --deploy
hexo deploy --generate
hexo deploy -g
hexo server -g

## 草稿
hexo publish [layout] <title>

## 模版
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub

hexo new [layout] <title>
hexo new photo "My Gallery"
hexo new "Hello World" --lang tw

最近在用hexo 搭建github pages 时，遇到一个问题，
hexo安装没错，也能成功运行。启动也没错。
就是不能访问。。
原因是：
你的电脑端口被占用了。
hexo默认的端口是4000，如果你的电脑安装了福昕阅读器，，就是他，没错，坑爹吧！！！！