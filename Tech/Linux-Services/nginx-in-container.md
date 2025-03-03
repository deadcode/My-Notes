# Start NGINX in a container

## Download the nginx image
``` shell
docker pull nginx
```
## Run nginx
``` shell
docker run --name docker-nginx -p 8080:80 nginx 
```
  - run is the command to create a new container
  - The --name flag is how you specify the name of the container. If left blank, a generated name like nostalgic_hopper will be assigned.
  - -p specifies the port you are exposing in the format of -p local-machine-port:internal-container-port. In this case, you are mapping port :80 in the container to port :80 on the server.
  - nginx is the name of the image on Docker Hub.

## Build a web page to serve 
``` shell
mkdir -p ~/docker-nginx/html

cd ~/docker-nginx/html

cat <<EOF >index.html
<html>
  <head>
    <title>Sandeep Dahiya's Homepage</title>
  </head>

  <body>
    <div class="container">
      <h1>Welcome to Sandeep's Homepage.
      Use the links below to access whatever you need.
      </h1>
      <hr>
      <h2>
      <p>Qosmos Protobooks</p>
      </h2>
      <li>
        Protobook 1.750.2: <a href=Qosmos_Protocol_Bundle_Protobook_1.750.2-20/index.html>here</a>
      </li>
      <hr>
      <h2>
        Device Images
      </h2>
      <li>
        Images folder: <a href=images/>here</a>
      </li>
    </div>
  </body>
</html>
EOF
```

## Create a custom nginx config file
  Could be used to enable autoindex for local directories 
  or enable PHP support
```shell
cd ~/docker-nginx

docker cp docker-nginx:/etc/nginx/conf.d/default.conf default.conf

cat <<EOF >default.conf
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        autoindex on;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
EOF
```
## Stop / Remove old container
```shell
docker stop docker-nginx

docker rm docker-nginx
```
## Restart docker nginx with all mapping
``` shell
docker run --name docker-nginx -p 80:80 -v ~/docker-nginx/html:/usr/share/nginx/html -v ~/docker-nginx/default.conf:/etc/nginx/conf.d/default.conf -d nginx
```
  - -d : Run docker in detached / daemon mode
  - -v : Map local file/directory to the container file/directory

## Note: After any changes to config file restart the nginx
``` shell
docker restart docker-nginx
```
