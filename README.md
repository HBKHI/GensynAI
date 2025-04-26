# GensynAI
All you need to know about contributing to GensyAI using VPS only
![image](https://github.com/user-attachments/assets/d663b050-b750-40d6-8f6a-068bfc8b7eb9)


## Hardware Requirements
CPU: Minimum >= 16GB RAM (more RAM for larger model or dataset execution)

Note: You can run the node without a GPU.

## VPS Renting
Start by purchasing an appropriate VPS on [Contabo](https://contabo.com/en/vps/) or any other server hosting platform. 

**Selecting VPS type**
1. Select the type of VPS specs, payment plan, region and storage type you please.

2. Leave the remaining default options as they are and either generate a password or input your password and save it.

3. After payment completion, you would get the ip address, password and port sent to your mail where you can access said VPS.

4. Download [Termius](https://termius.com/download) from their official website.

5. Create new connection, input vps ip address, and password (default port should be 22 and username should be root)

---

## 1) Install Dependencies
**1. Update System Packages**
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
**2. Install General Utilities and Tools**
```bash
sudo apt install screen curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

**3. Install Python**
```bash
sudo apt-get install python3 python3-pip python3-venv python3-dev -y
```

**4. Install Node**
```
sudo apt-get update
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
node -v
sudo npm install -g yarn
yarn -v
```

**5. Install Yarn**
```bash
curl -o- -L https://yarnpkg.com/install.sh | bash
```
```bash
export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"
```
```bash
source ~/.bashrc
```

---

## 2) Get HuggingFace Access token (not compulsory)
**1- Create an account in [HuggingFace](https://huggingface.co/)**

**2- Create an Access Token with `Write` permissions [here](https://huggingface.co/settings/tokens) and save it compulsorily**

---

## 3) Clone the Repository
```bash
git clone https://github.com/gensyn-ai/rl-swarm/
cd rl-swarm
```

---

## 4) Run the swarm
Open a screen to run it in background
```bash
screen -S swarm
```
Install swarm
```bash
python3 -m venv .venv
source .venv/bin/activate
./run_rl_swarm.sh
```
Press `Y`

---

## 5) Login
**1- You have to receive `Waiting for userData.json to be created...` in logs**

**2- Open login page in browser**
You have to forward port by entering a command in their local pc powershell command prompt.

**3- Forward port:**
* In windows start menu, Search **Powershell** and run as administrator in your local PC
* Enter the command below and replace your vps ip with `Server_IP` and your vps port(.eg 22) with `SSH_PORT`
```
ssh -L 3000:localhost:3000 root@Server_IP -p SSH_PORT
```
* Make sure you enter the command in your own local Windows Powershell cmd and NOT your VPS terminal.
* This prompts you to enter your VPS password (it wont be visible), when you enter it, you connect and tunnel to your vps
* Now go to browser and open `http://localhost:3000/` and login

**4- Login with your preferred method**

* After login, your terminal starts installation.

**5- Optional: Push models to huggingface**
* Enter your `HuggingFace` access token you've created when it prompted
* This will need `2GB` upload bandwidth for each model you train, you can pass it by entering `N`
---

### Node Name
* Now your node started running, Find your name after word `Hello`, it would essentially be three words, for example `leaping tenacoius dog`
---

### Screen commands
* Minimize: `CTRL` + `A` + `D`
* Return: `screen -x swarm`
* Reattach: `screen -r swarm`
* Stop current swarm execution: `CTRL` + `C`
* Terminate current swarm execution: `CTRL` + `D`

Alternatively:
* Stop and Kill: `screen -XS swarm quit`

---

# Node Health
### Official Dashboard
* Top 100 round-participants: https://dashboard.gensyn.ai/
<img width="1505" alt="image" src="https://github.com/user-attachments/assets/093b0954-275d-4c64-9ec3-19c8486e1328" />


### Telegram Bot
Search you `Node ID` here with `/check` here: https://t.me/gensyntrackbot 
* `Node-ID` is near your Node name

* If receiving `EVM Wallet: 0x0000000000000000000000000000000000000000`, your `onchain-participation` is not being tracked and you have to Install with `New Email` and ***Delete old `swarm.pem`*** by simply going to root folder and `rm -rf rl-swarm`

Alternatively
Use [this](https://gensyn-node.vercel.app/) site to check your node statistics.

---

# Troubleshooting:

### Upgrade viem & Node version in Login Page
1- Modify: `package.json`
```bash
cd rl-swarm
nano modal-login/package.json
```
* Update: `"viem":` to `"2.25.0"`

2- Upgrade
```bash
cd rl-swarm
cd modal-login
yarn install

yarn upgrade && yarn add next@latest && yarn add viem@latest

cd ..
```

### Unable to access forwarded port gensyn site
If you are ever in a situation where you need to restart your swarm and you cannot open localhost:3000 even after forwarding.
1- Open the browser where you opened the site
2- Clear cache history for a day
3- Re open localhost:3000 and you should be able to login back

### Ran out of input
On your root folder, navigate:
```
cd rl-swarm
```
Edit:
```
nano hivemind_exp/configs/mac/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
```
* Lower `max_steps` to `10`
* If issue persists,  Lower `max_steps` to `5`
