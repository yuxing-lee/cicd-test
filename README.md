# CI/CD-test

# Ultimate Goal

- [ ] CI/CD tools
    - [x] drone
    - [ ] gitlab ci 
    - [ ] jenkins 
    - [ ] travis ci
    - [ ] circleci 
- [ ] git repo
    - [x] github
    - [ ] gitlab
    - [ ] gitea
    - [ ] bitbucket
- [ ] lint
    - [ ] python
    - [ ] javascript
- [ ] test
    - [ ] python
    - [ ] javascript
- [ ] build docker image
    - [ ] docker hub
    - [ ] private docker repository
    - [x] github docker registry
- [ ] deploy 
    - [ ] host machine
    - [ ] remote machine
    - [ ] kubernetes
        - [ ] minikube
- [ ] notification
    - [ ] slack

# Flow

```mermaid
graph LR;
    git_action-->lint;
    subgraph CI
        lint-->test;
        test-->build;
    end
    subgraph CD
        build-->deploy;
        deploy-->notification;
    end
    test -- fail --> notification;
    lint -- fail --> notification;
    build -- fail --> notification;
    deploy -- fail --> notification;
```

# About Drone 

## Drone Server

1. collect git repo action
2. trigger drone runner

## Drone Runner

### Usage Scenarios

1. docker runner
    - every step is standalone docker container in pipeline
2. exec runner
    - run pipeline in host machine
3. ssh runner
    - run pipeline in remote machine
4. VM runner
    - run pipeline in VM

# Build Environment

## Install Docker

1. install docker & docker-compose
    - execute install/install_docker.sh
2. register [ngrok](https://dashboard.ngrok.com/) account and get auth token
3. install ngrok and run 
    ```bash
    wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
    tar -zxvf ngrok-v3-stable-linux-amd64.tgz
    ngrok config add-authtoken <your_token>
    ngrok http 80
    ```
4. github developer setting
    - [add new oauth app](https://github.com/settings/developers)
    - settings
        - Application name
        - Homepage URL：https://<ngrok_url>
        - Authorization callback URL：https://<ngrok_url>/login
5. modify drone/docker-compose.yml
    - DRONE_SERVER_HOST="{YOUR_MACHINE_HOST}"
    - DRONE_GITHUB_CLIENT_ID="{CLIENT_ID}"
    - DRONE_GITHUB_CLIENT_SECRET="{CLIENT_SECRET}"
5. use docker-compose run drone-server 
    ```bash
    cd drone
    sudo docker-compose up -d
    ```
6. authorize drone
    - open https://<ngrok_url>
7. successfully login drone

## Install kubernetes Node

### Install kubectl
```bash
sudo apt-get update && sudo apt-get install -y curl wget virtualbox apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

### Install minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version
```

### Start minikube
```bash
minikube start
kubectl cluster-info
```

# Reference

- [Install docker/docker-compose](https://www.51cto.com/article/715086.html)
- [How to setup drone environment](https://medium.com/starbugs/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E5%AD%B8-devops-%E9%82%A3%E5%B0%B1%E9%81%B8%E6%93%87%E6%9C%80%E7%B0%A1%E5%96%AE%E7%9A%84-drone-ci-%E9%96%8B%E5%A7%8B%E5%90%A7-931126671139)
- [Install minikube](https://www.learnitguide.net/2023/04/how-to-install-minikube-on-ubuntu-2204.html)
- [Github Docker combine with Drone](https://blog.wu-boy.com/2019/09/upload-docker-image-to-github-registry-using-drone/)
