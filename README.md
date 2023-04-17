
## 简介


**DouYin-Web**，是抖音后台管理系统前端项目

> 拥有用户管理、部门管理、岗位管理、菜单管理、角色管理、字典管理、参数管理、通知公告、操作日志、登录日志、在线用户、定时任务、系统接口、服务监控、连接池监视等功能。




## 开始使用

1. 环境准备
   * 运行启动【抖音后台管理系统】
   * 安装[node](http://nodejs.org/)和[git](https://git-scm.com/)



2. 本地开发

   进入项目根目录

   ```shell
   npm install
   ```

   > 若耗时太长可使用`npm install --registry=https://registry.npm.taobao.org`

   ```shell
   npm run serve
   ```

   > 打开浏览器访问 [http://localhost:8000](http://localhost:8080/)

3. 部署

   打包
 
     ```shell
     npm install
     ```
   打包完成后将/dist整个文件夹放入服务器中自定义目录(当然也可以在服务器上直接装环境拉代码后打包)。

   
   修改nginx配置：
   ```
         server {
        # 你的域名
          server_name sys.douyin.com;
          index index.html index.htm;
        # 你的打包路径
          root /data/douyin-web/dist;
        # gzip 加速
          gzip on;
          gzip_static on;
          gzip_min_length 1k;
          gzip_comp_level 9;
          gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/    gif image/png;
          gzip_vary on;
          gzip_disable "MSIE [1-6]\.";
          location / {
              try_files $uri $uri/ /index.html =404;
          }
          location /api/ {
          # 你的后台地址
              proxy_pass http://127.0.0.1:8080/;
              #proxy_set_header Host $http_host;
              client_max_body_size    20m;
              client_body_buffer_size 1280k;
              proxy_connect_timeout 15s;
              proxy_send_timeout 15s;
              proxy_read_timeout 15s;
              proxy_set_header X-Real-IP $remote_addr;
              proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          }
      }
   ```


