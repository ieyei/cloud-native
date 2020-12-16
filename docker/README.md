



https://docs.docker.com/develop/develop-images/dockerfile_best-practices/


git clone https://github.com/ieyei/cloud-native.git


## How to build docker image


```
cd docker/example
tree
├── Dockerfile
└── html
    └── somefile.html
```

A simple `Dockerfile` can be used to generate a new image that includes the necessary content:  
Dockerfile
```
FROM nginx
COPY ./html /usr/share/nginx/html
```

Place this file in the same directory as your directory of content ("html"), run `docker build -t myimage .`, then start your container:
```
docker build -t myimage:latest .
```

Start your container with exposing external port:
```
docker run --name my-nginx -d -p 8080:80 myimage
```

Then you can hit `http://host-ip:8080/somefile.html` in your browser.

