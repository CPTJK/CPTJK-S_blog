# 前端页面nginx配置示例

```shell
http {
    # gzip 配置
    # 启用gzip
    gzip on;
    
    # 设置gzip压缩级别（1-9）
    gzip_comp_level 5;
    
    # 设置需要压缩的最小文件大小 字节为单位
    gzip_min_length 256;
    
    # 设置gzip压缩的文件类型
    gzip_types
        text/plain
        text/css
        text/javascript
        application/javascript
        application/json
        application/xml
        application/x-font-ttf
        application/x-font-opentype
        image/svg+xml;
    
    # 禁止对IE6及以下版本使用gzip
    gzip_disable "MSIE [1-6]\.";
    
    # 添加Vary: Accept-Encoding头
    gzip_vary on;



    upstream backend_servers {
        server backend1.example.com:8080;
        # 如果有多个后端服务器，可以添加更多的 server 行
        # server backend2.example.com:8080;
        # server backend3.example.com:8080;
    }


    server {
        listen 80;
        server_name example.com;
        root /var/www/html;  # 请替换为您的实际前端文件路径

        index index.html;


        # API 转发配置
        location ^~ /api/ {
            proxy_pass http://backend_servers/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        # 对 index.html 的特殊处理
        location = /index.html {
            add_header Cache-Control "no-cache, no-store, must-revalidate";
            add_header Pragma "no-cache";
            add_header Expires "0";
            try_files $uri $uri/ =404;
        }

        # SPA 配置
        location / {
            try_files $uri $uri/ /index.html;
        }

        # 静态资源缓存设置
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
            expires max;
            add_header Cache-Control public;
        }

        # 可选：禁止访问 . 开头的隐藏文件
        location ~ /\. {
            deny all;
        }
    }
}
```