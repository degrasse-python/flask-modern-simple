# How to build and run a flask-api application as a container on local and within a kubernetes container orchestrator

This repo is an overview of how to deploy docker to two different envs. 
In the modern cloud example you will use a MacOS client to deploy to GKE. There is more work than the local dev environment example but still fun stuff.

If you are on a linux based distro as a client you need research each step to prepare your environment. There are plenty of docs that can help you with the environment setup. After you auth to the k8s environment then the steps will be the same across any client environment.

## Local MacOS Client: Development -> Build & Deploy
The steps below are how to prep your environment and run a container in your local development environment.

### Prereq's
Follow the steps below in sequence to clone, build, and deploy on your local MacOS. Below is the outline of everything you will do. Below that are the commandline steps. 

1. Install Brew
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
# install Docker 🤓
brew cask install docker
# check install
docker --version
# create 
```

Create a docker account. It's pretty straigt forward. After creating an account you need to create a public repo for building images and storing them in a repo.
[Follow these instructions](https://docs.docker.com/docker-hub/quickstart/) or reach out if you need help.
If you have issues with permissions create an access token and use that to login to docker.
```bash
docker login
```

**Build and Deploy:MacOS**

First you will clone the codebase and build the image. 
Then, change your directory to the working directory for the project. 
These commands will clone the codebase, build the image, and push the image for use in public cloud environments and local environments.

```bash
# clone code
git clone && cd 
# start the docker application
open -a docker
# build image for use with containers 🤓
## <repo-name> == the name of the repo that you created using your account.
## <image-name> == the name of your image built (e.g. flask-api).
## <image-version> == the version (e.g. 0.0.1 or 'alpine')
docker build -t '<repo-name>/<image-name>:<image-version>' .
# push the image to repo
docker push '<repo-name>/<image-name>:<image-version>'
# run the image for on your local
docker run -it --rm --name flask -p 5555:5555 <repo-name>/<image-name>:<image-version>
# check 0.0.0.0:5555 to see if the application works.

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
7. Create a kubernetes cluster on the Google Kubernetes Engine. 😎
8. Authenticate with you new cluster and check to make sure you are on the cluster for the install.
9. Use the k8s declaritive files for 


**Install requirements:MacOS**

Install brew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install Docker. If you are having issues visit this [link](https://docs.docker.com/desktop/install/mac-install/) for instructions.
```bash
# use brew to install docker
brew cask install docker
# check install
docker --version
# create 
```

Install [Google Cloud CLI](https://cloud.google.com/sdk/docs/install) 

Before initializing the cli you need to login to GCP and follow the [create a project link](https://cloud.google.com/resource-manager/docs/creating-managing-projects) to use for this example.
```bash
# download the correct file for your MacOS and unzip the file -- example below of unzip -- or use the UI
cd path/of/file
tar -xvf google-cloud-cli-darwin-x86_64.tar.gz
# cd into folder and run the script. Follow the prompts for the install.
./google-cloud-sdk/install.sh
# initialize the cli with your account and project
## MAKE SURE gcloud is in your PATH to use the gcloud command.
## follow the options in the init command
./google-cloud-sdk/bin/gcloud init
source ~/.zshrc
gcloud --version
```

Install [kubectl CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#install-with-homebrew-on-macos) for k8s.
```bash
brew install kubectl
kubectl version --client
# install gcloud plugin - https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl
gcloud components install gke-gcloud-auth-plugin
# check install
gke-gcloud-auth-plugin --version

```



**Build and Deploy:GKE** 😎

Clone the codebase and build the image. 
Then, change your directory to the working directory for the project. 
These commands will clone the codebase, build the image, and push the image for use in public cloud environments and local environments.

```bash
# clone code
git clone && cd 
# build image for use with containers 🤓
## <repo-name> == the name of the repo that you created using your account.
## <image-name> == the name of your image built (e.g. flask-api).
## <image-version> == the version (e.g. 0.0.1 or 'alpine')
docker build -t '<repo-name>/<image-name>:<image-version>' .
# push the image to repo
docker push '<repo-name>/<image-name>:<image-version>'
```

Create a kubernetes cluster on GCP, aka GKE. After creating the cluster you need to auth with the cluster to change the state of the cluster. This will allow us to deploy application and secure our cluster.

[Follow these instructions to get started with GKE](https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-zonal-cluster) or reach out if you need help.

Here you will provision a k8s cluster using gcloud CLI and then auth to the cluster using kubectl CLI. 
```bash
# update components for GCP
gcloud components update
# create cluster -- example below command --  use UI if you have issues
gcloud container clusters create CLUSTER_NAME \
  --zone COMPUTE_ZONE 
  --release-channel "regular" \
  --cluster-ipv4-cidr "/17"

## example
gcloud container clusters create example-cluster \
  --zone us-central1-a \
  --release-channel "regular" \
  --cluster-ipv4-cidr "/17"

# get creds for cluster to perform work against instance 😎
gcloud container clusters get-credentials CLUSTER_NAME \
  --region=COMPUTE_REGION

# install helm
brew install helm
# check
helm -h
```


Lastly you will use the kubectl CLI with the k8s manifests to build out your application architecture on the container orchestrator. Links to supporting documentation will be included in the comments of the commands. 

```bash
# auth with cluster. Login to UI and grab this if you have issues.
gcloud container clusters get-credentials <cluster-name> \
  --region <region> \
  --project <project>
# check the name of the cluster
kubectl config current-context

# make new namespaced networks
kubectl create namespace ingress-nginx
kubectl create namespace apis

# add repos
helm repo update
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo add jetstack https://charts.jetstack.io
# creat the loadbalancer
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --set controller.publishService.enabled=true \
  -n ingress-nginx --create-namespace
```

Modify the following lines of of each manifest to correctly deploy your application.

`cd /path/to/localcode`

```bash
/k8s/flask-deployment.yaml
      # add the correct image 
ln17: image: <repo-name>/<image-name>:<image-version>

```

```bash
# deploy the application
kubectl apply -f k8s/flask-deploy.yaml
# expose the container port
kubectl apply -f k8s/flask-srv.yaml
# map the srv to the loadbalancer
kubectl apply -f k8s/flask-ingress.yaml

# assign the IP
INGRESS_IP=$(kubectl get services -n ingress-nginx | grep LoadBalancer  | awk '{print $4}')
# get the IP
echo $INGRESS_IP

```

Copy / Paste the IP from your terminal and check to see if it's working via your web browser!

**🏁🏁🏁🏁🏁🏁🏁**