# CI/CD-test

# build environment

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

# reference

- [Install docker/docker-compose](https://www.51cto.com/article/715086.html)
- [How to setup drone environment](https://medium.com/starbugs/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E5%AD%B8-devops-%E9%82%A3%E5%B0%B1%E9%81%B8%E6%93%87%E6%9C%80%E7%B0%A1%E5%96%AE%E7%9A%84-drone-ci-%E9%96%8B%E5%A7%8B%E5%90%A7-931126671139)