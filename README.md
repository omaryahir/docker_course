# Docker Course 

### Few first command(s)
$ docker run hello-world 
$ docker run busybox echo "hello from busybox"

### List all docker containers
$ docker ps -a 

### Connect to sh terminal of the container
$ docker run -it busybox sh 

### Running a simple static server 
$ docker run -d -P --name static-site prakhar1989/static-site 
                     ^ name we want to give 
                 ^ this will publish all exposed ports to random ports
              ^  detach our terminal 

### Check ports of a container 
$ docker port <container_name>
$ docker port static-site 

### Stop a container 
$ docker stop <container_name>
$ docker stop static-site 

### Specifying custom ports 
$ docker run -p 8888:80 prakhar1989/static-site 

### Get specific image version 
$ docker pull ubuntu:18.04 

### Dockerfiles

They have to be named: Dockerfile 

Content Example:
```
FROM python:3
# set a directory for the app
WORKDIR /usr/src/app   
# copy all the files to the container 
COPY . .               
# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt 
EXPOSE 5000 
CMD ["python", "./app.py"]
```

In order to build our image we need to run this command in the same directory of the Dockerfile:
$ docker build -t yourusername/test .
                                    ^ path where Dockerfile is.
                ^ custom tag name 


# Docker Compose - docker-compose.yml file 
version: "3"
services:
  es:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
    container_name: es 
    environment:
      - discovery.type=single-node
    ports:
      - 9200:9200
    volumes:
      - esdata1:/usr/share/elasticsearch/data
  web:
    image: omaryahir/myflaskprojectweb
    command: python app.py 
    depends_on:
      - es
    ports:
      - 5000:5000 
    volumes:
      - ./flask-app:/opt/flask-app 
volumes:
  esdata1:
    driver:local 






