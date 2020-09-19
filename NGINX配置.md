    user  root;
    worker_processes  1;
    
    
    events {
        worker_connections  1024;
    }
    
    
    http {
        include       mime.types;
        default_type  application/octet-stream;
    
    
        sendfile        on;
        #tcp_nopush     on;
    
        #keepalive_timeout  0;
        keepalive_timeout  65;
    
        #gzip  on;
    
        server {
            listen       80;
            server_name  mhw828.com;
            return 301 https://$host$request_uri; 
        }
    
        server {
            listen 443 ssl;
            #填写绑定证书的域名
            server_name cloud.tencent.com; 
            #证书文件名称
            ssl_certificate 1_www.mhw828.com_bundle.crt; 
            #私钥文件名称
            ssl_certificate_key 2_www.mhw828.com.key; 
            ssl_session_timeout 5m;
            ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
            ssl_prefer_server_ciphers on;
    
            location / {
                #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
                    root   /home/git/projects/blog;
                    index  index.html index.htm;
            }
    
            location /ws/ {
                #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
                    add_header 'Access-Control-Allow-Origin' "$http_origin" always;
                    add_header 'Access-Control-Allow-Credentials' 'true' always;
                    add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS' always;
                    add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-  Since,Keep-Alive,Origin,User-Agent,X-Requested-With' always;
    
    
                    proxy_pass http://localhost:9998/;
                    proxy_set_header Host   $host;
                    proxy_set_header X-Real-IP      $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    
                    proxy_read_timeout   10000s;
                    proxy_http_version 1.1;
                    proxy_set_header Upgrade $http_upgrade;
                    proxy_set_header Connection "upgrade";
            }
    
        }
    }