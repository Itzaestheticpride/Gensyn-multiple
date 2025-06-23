## Proper guide how to run your multiple nodes on single system

```bash
wget https://raw.githubusercontent.com/Itzaestheticpride/Gensyn-multiple//main/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
```

# Make sure you have enough ram and cores for the other nodes too 
# Requirements 
1. swarm.pem
2. userdata.json ( can be found in your $HOME/rl-swarm/modal-login/temp-data/)
3. userapi.json hivemind_exp/configs/mac/grpo-qwen-2.5-0.5b-deepseek-r1.yaml 


lets make a new user profile on the server 
```bash
sudo useradd swarm
```
# here swarm is username i have createtd you can choose your own. I will choose swarm as a username from now make sure you use yours if you change it 
move to the profile 
```bash
cd /home/swarm/
```
# Lets make a dir name gensyn-node
```bash
mkdir gensyn-node
```
# Run to download some necessary files
```bash
sudo apt-get update && sudo apt-get upgrade -y && curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash &&
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl screen git yarn && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && sudo apt update && sudo apt install -y yarn &&
apt-get update && apt-get install -y build-essential
```


# cd into the dir 
```bash
cd gensyn-node
```
# create a docker compose file 
```bash
nano docker-compose.yml
```
# paste the following into the file
```bash
version: "3.9"

services:
  gensyn-node:
    image: python:3.12-slim
    container_name: gensyn-node-user
    volumes:
      - ./rl-swarm-0.4.3:/app
      - ./config/userdata.json:/app/modal-login/temp-data/userdata.json:ro
      - ./config/api.json:/app/modal-login/temp-data/api.json:ro
    working_dir: /app
    command: >
      bash -c "apt-get update &&
               apt-get install -y git &&
               apt-get update && apt-get install -y curl
               curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash
               python3 -m venv .venv &&
               . .venv/bin/activate &&
               ./run_rl_swarm.sh"
    tty: true
    stdin_open: true
    restart: unless-stopped
    
```

# To exit press ctrl+x tthen y then enter

#clone the dir. (This files works only for the cpu based servers)
```bash
wget https://github.com/gensyn-ai/rl-swarm/archive/refs/tags/v0.4.3.tar.gz && tar -xvzf v0.4.3.tar.gz && cd rl-swarm-0.4.3
```
```bash
sudo rm -rf run_rl_swarm.sh && sudo rm -rf hivemind_exp/configs/mac/grpo-qwen-2.5-0.5b-deepseek-r1.yaml/ && 
wget https://raw.githubusercontent.com/Itzaestheticpride/node-0.4.2/main/run_rl_swarm.sh && 
wget https://raw.githubusercontent.com/Itzaestheticpride/node-0.4.2/main/grpo-qwen-2.5-0.5b-deepseek-r1.yaml 
```
# files are ready lets go wth docker 
```bash
cd ..
```
# run the dockerfile
```bash
apt install docker-compose -y  && docker-compose up 
```
## provide feedback please if you face some type of error @rahuljaat2502 tele

