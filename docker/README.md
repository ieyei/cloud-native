



https://docs.docker.com/develop/develop-images/dockerfile_best-practices/


git clone https://github.com/ieyei/cloud-native.git


## Build and run your image

```
cd docker/example
tree
├── Dockerfile
└── html
    └── somefile.html
```

### Building Docker images
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

### Running a Docker image
Start your container with exposing external port:
```
docker run --name my-nginx -d -p 8080:80 myimage
```

Then you can hit `http://host-ip:8080/somefile.html` in your browser.

## Share images on Docker Hub
The Docker image you built still resides on your local machine. This means you can’t run it on any other machine outside your own. To make the Docker image available for use elsewhere, you need to push it to a Docker registry.

You need only to log in with your username and password.
```
docker login
```
Retag the image with a version:
```
docker tag myimage:latest loovee/myimage:latest
```
Then push the image to docker hub
```
docker push loovee/myimage:latest
```

## Links
[Docker Getting Started](https://docs.docker.com/get-started/)