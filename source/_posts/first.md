---
title: ·centos7搭建hexo博客步骤·
date: 2022-05-17 16:35:06
tags: Centos
author: Jerry
---

# centos7搭建hexo博客步骤

### 安装插件

#### **1、nodejs**

关于linux系统的版本在`https://github.com/nodesource/distributions`都有记录如何安装

这里使用的是**CentOS 7** (64-bit)系统，使用的**Node.js**是14版本


<: 获取软件源

```
curl -fsSL https://rpm.nodesource.com/setup_14.x | bash -
```


<: 安装nodejs

```
Run `sudo yum install -y nodejs` to install Node.js 14.x and npm
```

<: 版本如下

```
[root@localhost ~]# node -v
v14.19.1
[root@localhost ~]# npm -v
6.14.16
```

#### **2、git**

```
sudo yum install -y git-core
```

#### **3、安装cnpm**

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
验证是否安装成功
[root@localhost ~]# cnpm -v
cnpm@7.1.1 (/usr/lib/node_modules/cnpm/lib/parse_argv.js)
npm@6.14.16 (/usr/lib/node_modules/cnpm/node_modules/npm/lib/npm.js)
node@14.19.1 (/usr/bin/node)
npminstall@5.8.1 (/usr/lib/node_modules/cnpm/node_modules/npminstall/lib/index.js)
prefix=/usr
linux x64 3.10.0-1160.el7.x86_64
registry=https://registry.npmmirror.com
```

### 安装并初始化博客

```
#安装hexo
npm install -g hexo
#在当前目录创建文件夹boke并初始化
hexo init ./boke
#使用国内的镜像为你完成博客的初始化工作
cnpm install
#清空缓存
hexo cl
#启动博客
hexo s
```

### 更换主题

 [hexo主题官方地址](https://hexo.io/themes/)

 [本次使用](https://github.com/blinkfox/hexo-theme-matery)

```
下载主题（在工作目录下执行命令）
cd themes && mkdir 主题名字 && git clone https://github.com/blinkfox/hexo-theme-matery
```

**两个重要文件**

 #站点配置文件：./*config.yml*

 #主题配置文件：./themes/主题名字/_config.yml

**站点配置文件：**

```
# Site
title: boke         #标题     
subtitle: ''
description: ''
keywords:
author: 哆啦C梦       #作者
language: zh-CN      #语言
timezone: ''

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next         #使用的主题

```

**主题配置文件**（可更改主题的风格等）

```
# Schemes                    #主题风格
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini

busuanzi_count:                #显示阅读量
  enable: true
  total_visitors: true
  total_visitors_icon: fa fa-user
  total_views: true
  total_views_icon: fa fa-eye
  post_views: true
  post_views_icon: fa fa-eye
```

### 生成文章

```
#hexo new "文章链接"   文章链接建议是英文，每次生成的文章都固定在你的博客根目录下面的source/_posts下 ，文章链接为中文会乱码。（在博客根目录下面执行）
hexo new "cc1.md"
#编辑文件，按照目录打开cc1.md文件，可以看到一个横线上面有一段信息
---
title: cc1       文章标题
date: 2021-08-07 09:36:20     文章日期
tags:             文章标签
---
#然后这时候你可以修改文章标题，并且在横线下面输入你文章的内容
```

### 在文章中插入图片

**绝对路径**
当Hexo项目中只用到少量图片时，可以将图片统一放在source/images文件夹中，通过markdown语法访问它们 /images/image.jpg

```
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-c2uLb1qm-1630293337065)(/images/image.jpg)]
```

图片既可以在首页内容中访问到，也可以在文章正文中访问到。

**相对路径**
图片除了可以放在统一的images文件夹中，还可以放在文章自己的目录中。文章的目录可以通过配置_config.yml来生成。

```
vim  _config.yml
	post_asset_folder: true
```

将_config.yml文件中的配置项post_asset_folder设为true后，执行命令$ hexo new post_name，在source/_posts中会生成文章post_name.md和同名文件夹post_name。将图片资源放在post_name中，文章就可以使用相对路径引用图片资源了。

```
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-WtVxylqL-1630293337066)(image.jpg)]
```

上述是markdown的引用方式，图片只能在文章中显示，但无法在首页中正常显示。

如果希望图片在文章和首页中同时显示，可以使用标签插件语法。

```
#插入图片时用这种方式：
{% asset_img test.jpg This is an test image %}
其中test.jpg就是你要引用的图片，我这里就是test.jpg，后面的This is an test image是图片描述，可以自己修改。
```

**使用图床**

将图片放在部署的图床内，通过链接的方式供文章使用

如：

```
![a3.png](https://img-blog.csdnimg.cn/img_convert/f8a7f7a962c868719c4005547460cf42.png)
```

### 部署到gitee

```
#生成密钥，密钥位置：/root/.ssh/id_rsa.pub
ssh-keygen
#将密钥添加到gitee，点击头像——设置——安全设置——SSH公钥——把生成的公钥复制进去即可
```

**在gitee中创建项目**

 设置git信息

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

**打开站点配置文件**，修改deploy信息

```
deploy:
  type: git
  repo: 你复制的地址SSH类型
  branch: master
```

**安装上传插件**

```
cnpm install hexo-deployer-git --save
```

**上传**

```
#在工作目录下
hexo g -d
```

执行命令：hexo s 即可查看页面内容

### 将博客部署到nginx

因为hexo s 命令所展示的页面极为不稳定，容易掉线，因此将博客内容迁移至nginx很有必要。

```
#配置epel源
yum install epel-release
#yum安装
yum -y install nginx
#启动nginx
systemctl start nginx
#查找配置文件
find / -name *nginx*
# 编辑配置文件/etc/nginx/nginx.conf，并填写如图所示的内容.
vim /etc/nginx/nginx.conf

        location / {
            root   html/public/;
            index  index.html;
                   }
```

## 最后

每次修改博客后均需执行以下命令

```
cd /root/boke/
hexo g
hexo d
rm -rf /usr/local/nginx/html/public
cp -r public /usr/local/nginx/html/
/usr/local/nginx/sbin/nginx -s reload            #重新加载nginx,不同的安装方式所处的位置不同。
```


