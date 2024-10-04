# How to build and run a flask container on local and in modern cloud

This repo is an overview of how to deploy docker to two different envs. 
In the modern cloud example you will use a MacOS client to deploy to GKE. There is more work but still fun stuff.

If you are on a linux machine then you need to follow this is install docker.



## Local Development Build & Deploy
The steps below are how to prep your environment and run a container in your local development environment.

### Prereq's
Follow the steps below in sequence to deploy on your local MacOS. Below is the outline of everything you will do. Below that are the commandline steps. 

1. Install brew
2. Install Docker and [create an account.](https://docs.docker.com/docker-hub/quickstart/)
3. Create Docker Container Repo to store images (or use an existing one).
4. Clone git repo
5. Build image
6. Run container on local Docker

**Install requirements:MacOS**
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

**Build and Deploy:MacOS**
Create a docker account. It's pretty straigt forward. After creating an account you need to create a public repo for building images and storing them in a repo.
[Follow these instructions](https://docs.docker.com/docker-hub/quickstart/) or reach out if you need help.
```bash
docker login
```

Next you will clone the codebase and build the image. 
Then, change your directory to the working directory for the project. 
These commands will clone the codebase, build the image, and push the image for use in public cloud environments and local environments.
```bash
# clone code
git clone && cd 
# build image for use with containers
docker build -t '<repo-name>/<image-name>:<image-version>' .
# push the image to repo
docker push '<repo-name>/<image-name>:<image-version>'
# run the image for on your local
```


## Public Cloud Build & Deploy
The steps below are how to prep your environment and run a container in your local development environment.

### Prereq's
Follow the steps below in sequence build and deploy to GCP. The CLI commands may vary between different cloud providers. This example is for Google Cloud Platform. Below is the outline of everything you will do. Below that are the commandline steps. 

1. Install Brew
2. Install Docker and [create an account.](https://docs.docker.com/docker-hub/quickstart/)
3. Install [Google Cloud CLI](https://cloud.google.com/sdk/docs/install) and the [kubectl CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos) for k8s.
4. Create Docker Container Repo to store images (or use an existing one).
5. Clone Git repo
6. Build image locally and push to container repo.
7. Create a kubernetes cluster on the Google Kubernetes Engine.
8. Authenticate with you new cluster and check to make sure you are on the cluster for the install.
9. Use the k8s declaritive files for 


**Install requirements:MacOS**

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

Install [Google Cloud CLI](https://cloud.google.com/sdk/docs/install) 
```bash
# download the correct file for your MacOS and unzip the file -- example below of unzip

# cd into folder and run the script. Follow the prompts for the install.
./google-cloud-sdk/install.sh
# initialize the cli
./google-cloud-sdk/bin/gcloud init
```

Install [kubectl CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos) for k8s.
```bash
brew install kubectl
kubectl version --client
```



**Build and Deploy:MacOS**
Create a docker account. It's pretty straigt forward. After creating an account you need to create a public repo for building images and storing them in a repo.
[Follow these instructions](https://docs.docker.com/docker-hub/quickstart/) or reach out if you need help.

```bash
docker login
```

Next you will clone the codebase and build the image. 
Then, change your directory to the working directory for the project. 
These commands will clone the codebase, build the image, and push the image for use in public cloud environments and local environments.
```bash
# clone code
git clone && cd 
# build image for use with containers
docker build -t '<repo-name>/<image-name>:<image-version>' .
# push the image to repo
docker push '<repo-name>/<image-name>:<image-version>'
# run the image for on your local
```

Next you will start a k8s cluster using gcloud CLI and then auth to the cluster using kubecli. After that you will use the k8s yamls to build out your application on the container orchestrator. Links to supporting documentation will be included in the comments of the commands. 
```bash

```


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
