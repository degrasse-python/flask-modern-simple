# How to build and run a flask container on local and in modern cloud

This repo is an overview of how to deploy docker to two different envs. 
In the modern cloud example you will use a MacOS client to deploy to GKE. There is more work but still fun stuff.

If you are on a linux machine then you need to follow this is install docker.



## Local dev environment
The steps below are how to prep your environment and run a container in your local development environment.

### Prereq's
Follow the steps below in sequence to deploy on your local MacOS. Below is the outline of everything you will do. Below that are the commandline steps. 

1. Install brew
2. Install Docker and [create account](https://docs.docker.com/docker-hub/quickstart/)
3. Create Docker Container Repo to store images (or use an existing one).
4. Clone git repo
5. Build image
6. Run container on local Docker

**Install requirements**
Install brew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install Docker. If you are having issues visit this [link](https://docs.docker.com/desktop/install/mac-install/) for instructions.
```bash
brew cask install docker
# check install
docker --version
# create 
```

Create a docker account. It's pretty straigt forward. After creating an account you need to create a repo to build and push to.
[Follow these instructions](https://docs.docker.com/docker-hub/quickstart/) or reach out if you need help.
```bash
docker login
```

docker build -t "5thcinema/go-backend-services:k8s-interview" .


**Start Services-local:MacOS**
```bash
# start redis server
redis-server
# starts a local RabbitMQ node
brew services start rabbitmq
# start the celery workers
celery -A celery_task_app:worker worker -l info

```

**Start Services-local:Docker**
```bash

# https://www.rabbitmq.com/docs/download
docker run -d --name my-redis-stack -p 6379:6379  redis/redis-stack-server:latest
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:4.0-management
celery -A celery_task_app:worker worker -l info

```



## Modern Cloud environment
The steps below are how to prep your environment and run a container on GCPs GKE.

### Prereq's
Follow the steps below in sequence to deploy to GKE. Below is the outline of everything you will do. Below that are the commandline steps. 

1. Install brew
2. Install Docker
3. Clone git repo
4. Build image
5. Run container on local Docker
