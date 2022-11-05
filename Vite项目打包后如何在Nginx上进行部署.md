### 前言

在我的博客项目中，我使用的是Vite进行前端页面的构建，所以当我执行Vite Build命令后，将打包好的项目部署到服务器上时，遇到了一些之前没有遇到过问题，下面我将一步一步讲述，包括Vite项目的打包配置、以及Nginx服务器的配置。（特别说明：本文只针对Vite项目的Nginx部署，其他项目的部署只做类似参照。）

### Vite打包配置

在执行Vite Build命令之前，我们首先得先做好项目中的vite.config.ts/js中的配置，这里的配置主要和项目的打包相关。我们只需要在vite.config.ts/js中的defineConfig包裹的对象中要注意base值的设置，官方解释一般在开发环境时设置为'./'，但是在生产环境就要有所变化，具体和你在nginx下的conf.d配置，所以在设置base取值时，应该使用env来进行配置，具体代码如下图。

![vite.config配置](https://api.darwin.fun/images/serve?name=BLOG_02_IMG_01.png)

![vite.config配置](https://api.darwin.fun/images/serve?name=BLOG_02_IMG_02.png)

![vite.config配置](https://api.darwin.fun/images/serve?name=BLOG_02_IMG_03.png)

vite.config.ts/js中其他参数的配置，详情请见[vite官方文档](https://cn.vitejs.dev/config/#base)，这里就不做过多讲述。

### 将打包好的文件夹上传服务器

设置好vite.config.ts/js的配置后，在执行vite build命令后，正常情况下会在你在vite.config.ts/js中的配置的输出目录下，生成一个dist文件夹，这个文件夹就是你打包好后的项目，接下来你需要把dist文件夹上传至服务器上，具体上传方法windows用户建议选择[WinSCP](https://winscp.net/eng/index.php),mac用户建议选择自带的终端使用scp命令进行文件的上传命令如下。

``` ruby
scp -r local_folder remote_username@remote_ip:remote_folder
```

其中，local_folder是源地址，remote_username@remote_ip:remote_folder是目标地址。命令会将源地址的内容上传到目标地址。

### Nginx服务器配置

终于来到了关键的一步，首先我们要在nginx目录中（一般在根目录中的etc目录下）的conf.d目录中创建一个你将要部署的网站域名为名的.conf文件（例如我的博客域名为blog.darwin.fun，那么这里就应该命名为blog.darwin.fun.conf）。这个conf配置文件就是在nginx服务器被访问时，将会去该文件下配置好的路径下去寻找你的项目html文件,具体命令如下。

``` ruby
cd /etc/nginx/conf.d

sudo mkdir blog.darwin.fun.conf # blog.darwin.fun为我的网站域名，这里你应该换成你的

# 使用vim命令，编辑conf文件

sudo vim blog.darwin.fun.conf # blog.darwin.fun为我的网站域名，这里你应该换成你的

#按下i，进入编辑模式
```

![nginx.conf配置](https://api.darwin.fun/images/serve?name=BLOG_02_IMG_04.png)

具体Nginx配置如上图所示。

``` ruby
server {
  listen  80; # 这里是http服务请求的端口80
  server_name blog.darwin.fun; # blog.darwin.fun为我的网站域名，这里你应该换成你的
  return 301 https://$host$request_uri; # 这里是使用https协议，进行了重定向，如果你使用的http协议，这里请忽略，直接在该serve中配置下面的location
}

server {
  listen  443 ssl; # 这里是https服务请求的端口443，如果不使用https请求请忽略
  server_name  blog.darwin.fun; # blog.darwin.fun为我的网站域名，这里你应该换成你的
  client_max_body_size  200m;
  ssl_certificate  /etc/api_https_auth/blog.darwin.fun_bundle.crt;  # 这里是https协议的证书在服务器中的路径，如果你有证书的话，请按照此配置
  ssl_certificate_key  /etc/api_https_auth/blog.darwin.fun.key; # 这里是https协议的证书在服务器中的路径，如果你有证书的话，请按照此配置
  ssl_session_timeout   5m;
  ssl_protocols  TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
  ssl_ciphers  ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_prefer_server_ciphers  on;

  location / {   # 这里注意，这里的/就是前面你在vite.config.js/ts中配置的base路径。
    root /mnt/blog/dist; # 这里是你的dist文件夹在服务器中所在的路径
    index index.html index.htm; # 这是的index就是要去dist目录下寻找的index.html文件
    try_files $uri $uri/ /index.html; # 如果你在项目中使用路由模式是hash模式，请忽略，这里是使用history模式的配置，非常重要。
  }
}

# 按下ESC，退出编辑模式，输入:wq，按下回车键进行保存
```

到这一步，你的nginx服务已经配置好了，下面就要测试你的nginx配置是否可用，可以使用下面的命令进行配置。

``` ruby
sudo nginx -t # 该命令会进行测试你的conf文件是否配置正确
```

如果配置正确，会出现如图所示。

![nginx.conf配置](https://api.darwin.fun/images/serve?name=BLOG_02_IMG_05.png)

如果没有配置正确，请严格查看是否按照上述流程完成配置。

最后一步，重启nginx服务。

``` ruby
sudo systemctl reload nginx
```

重启完nginx服务后，你就可以去访问你的网站域名，我想应该会给你带来惊喜。

### 结束

这篇博客主要是讲述如何部署你的打包好的vite项目，其他的项目也可以参考上述流程，但具体肯定会有所差别，需要你去独立找寻答案。写到这已经凌晨三点半，明日还需工作，就写到这了，最后附上日常图片。

![每日一图](https://api.darwin.fun/images/serve?name=BLOG_02_IMG_06.jpg)