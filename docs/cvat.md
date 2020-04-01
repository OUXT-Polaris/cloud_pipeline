## Setup

```
$ sudo apt install python3-pip
$ sudo python3 -m pip install docker-compose
```

## installation

```
$ cd ~/docker/
$ git clone https://github.com/opencv/cvat
$ cd cvat/
$ docker-compose build
$ docker-compose up -d
$ docker exec -it cvat bash -ic 'python3 ~/manage.py createsuperuser'

$ docker ps -a
$ docker-compose stop
```

## Auto anotation
Use Tensorflow Object Detection

### Build

```
$ docker-compose -f docker-compose.yml -f components/tf_annotation/docker-compose.tf_annotation.yml build
```

### run

```
$ docker-compose -f docker-compose.yml -f components/tf_annotation/docker-compose.tf_annotation.yml up -d
```
