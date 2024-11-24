# Project description

It's a test task for DevOps Engineer. The task description at [dockerhub ReadMe](https://github.com/taptima/devops-test-questionnaire/blob/main/README.md) and [Notion](https://taptima.notion.site/Devops-9d61f75e266849cc9173ac6c70fc872c)

# Services dockerization

## Get source code of the both services

Get source code of the both services from github [devops-test-questionnaire](https://github.com/taptima/devops-test-questionnaire):

```bash
mkdir ./devops_test_workspace && cd ./devops_test_workspace
git clone https://github.com/taptima/devops-test-questionnaire
```

## Get source of Dockerfiles and k8s manifests

```bash
mkdir ./config_files
git clone https://github.com/malinat0r/DevOps-Test-Questionnaire ./config_files
```

## Build docker images

You can build docker images locally and pull them into minikube using instructions below or just use pre-built images from Docker Hub. In case you do not want to build images and want to use pre-built one just skip steps from this chapter ( Build docker images ).
```bash
# copy dockerfile into questionnaire-backend service source code folder
cp  ./config_files/docker/back/Dockerfile ./devops-test-questionnaire/questionnaire-backend/

# build docker image
docker build --no-cache --progress=plain -t malinovskij/questionnaire-backend:v1 ./devops-test-questionnaire/questionnaire-backend/

# Upload built image from docker to minikube
minikube image load malinovskij/questionnaire-backend:v1

# copy dockerfile into questionnaire-frontend service source code folder
cp  ./config_files/docker/front/Dockerfile ./devops-test-questionnaire/questionnaire-frontend/

# build docker image
docker build --no-cache --progress=plain -t malinovskij/questionnaire-frontend:v1 ./devops-test-questionnaire/questionnaire-frontend/

# Upload built image from docker to minikube
minikube image load malinovskij/questionnaire-frontend:v1
```

## Run Docker Compose

You can run the applicatiop in order to test built images in Docker compose. If do not want to do it, just skip this chapter.

```bash
docker compose --file ./config_files/docker-compose.yml up
```

Update your `/etc/hosts` file:

```bash
sudo echo "127.0.0.1 questionnaire.local" >> /etc/hosts
```
Open url in browser `"http://questionnaire.local/"` and test the application

Shut down docker compose
```bash
docker compose --file ./config_files/docker-compose.yml down
```

## Deploy to minikube

Assuming you have a minikube already installed. You can follow [these instructions](https://minikube.sigs.k8s.io/docs/start/) to set it up.
If you haven't already, install the Nginx Ingress Controller in your Minikube cluster.

```bash
# Enable ingress addon:
minikube addons enable ingress
```

Apply the application manifests
```bash
kubectl apply -f ./config_files/k8s/
```

Wait for ingress address:
```bash
kubectl get -n questionnaire-namespace ingress
NAME              CLASS   HOSTS   ADDRESS          PORTS   AGE
example-ingress   nginx   *       <your_ip_here>   80      5m45s
```

> Note for Docker Desktop Users:
To get ingress to work you’ll need to open a new terminal window and run `minikube tunnel` and in the following step use `127.0.0.1` in place of `<ip_from_above>`.


In case you installed minikube using Docker environment ( i.e. **NOT** Docker Desktop) then you need to adjust you `/etc/hosts` file with `<ip_from_above>`
```bash
abnet@LabNetVBoxStation:~$ kubectl get -n questionnaire-namespace ingress
NAME                         CLASS   HOSTS                 ADDRESS        PORTS   AGE
questionnaire-api-ingress    nginx   questionnaire.local   192.168.49.2   80      140m
questionnaire-main-ingress   nginx   questionnaire.local   192.168.49.2   80      140m
labnet@LabNetVBoxStation:~$

# cut from /etc/hosts
192.168.49.2 questionnaire.local
#127.0.0.1 questionnaire.local
```
Now you can start testing of application - open url in browser `"http://questionnaire.local/"`. 
