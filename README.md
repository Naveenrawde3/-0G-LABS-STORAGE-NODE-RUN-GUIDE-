# 😱😱 0G-LABS-STORAGE-NODE-RUN-GUIDE-BY-NTEK


## Node Management
  
## 🔄 Inter Gensyn Screen :

```bash
screen -r og
```

## Check screen sessions :

```bash
screen -ls
```

## Reattach to screen session :

```bash
screen -r
```

# Device/System Requirements 💻
![image](https://github.com/Naveenrawde3/-0G-LABS-STORAGE-NODE-RUN-GUIDE-/blob/main/photo_2025-06-23_19-25-27.jpg)

# Pre-Requirements 🛠

* Add 0G-Testnet-RPC : https://chainscan-galileo.0g.ai/

* Take faucet: https://faucet.0g.ai/


# Install All Require Dependecies

```
sudo apt-get update && sudo apt-get upgrade -y
```

```
sudo apt install curl iptables build-essential git wget lz4 jq make cmake gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```

* Install rustup

```
curl https://sh.rustup.rs -sSf | sh
```

```
source $HOME/.cargo/env
```

Check version

```
rustc --version
```


* Install go

```
wget https://go.dev/dl/go1.24.3.linux-amd64.tar.gz && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf go1.24.3.linux-amd64.tar.gz && \
rm go1.24.3.linux-amd64.tar.gz && \
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc && \
source ~/.bashrc
```

check version

```
go version
```

## 🖥 Creat Screen

```bash
screen -S og
```


# Clone the Repository

```
git clone https://github.com/0glabs/0g-storage-node.git
```

```
cd 0g-storage-node && git checkout v1.0.0 && git submodule update --init
```

* Build in release mode 

```
cargo build --release
```

# Set Configrations

```
rm -rf $HOME/0g-storage-node/run/config.toml
```

```
curl -o $HOME/0g-storage-node/run/config.toml https://raw.githubusercontent.com/Mayankgg01/0G-Storage-Node-Guide/main/config.toml
```


* Add Your Wallet's Private KEY in `config.toml`, ❗❗Dont Add **0X** before the key:

-open and go to `miner_key` and add your pvt key:

```
nano $HOME/0g-storage-node/run/config.toml
```

# If u want to change RPC then follow this:

1. get rpc from here - https://www.astrostake.xyz/0g-status

2. Chooose any rpc and edit in the `config.toml` file

# Create a Systemd Service File

```
sudo tee /etc/systemd/system/zgs.service > /dev/null <<EOF
[Unit]
Description=ZGS Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/0g-storage-node/run
ExecStart=$HOME/0g-storage-node/target/release/zgs_node --config $HOME/0g-storage-node/run/config.toml
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

* Reload

```
sudo systemctl daemon-reload
```

* Enable

```
sudo systemctl enable zgs
```

* Start service

```
sudo systemctl start zgs
```

# Managing Logs

```
sudo systemctl status zgs
```

* check Full Logs

```
tail -f ~/0g-storage-node/run/log/zgs.log.$(TZ=UTC date +%Y-%m-%d)
```

* Check block & Sync process - Match to the latest block on explorer

```
 while true; do     response=$(curl -s -X POST http://localhost:5678 -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"zgs_getStatus","params":[],"id":1}');     logSyncHeight=$(echo $response | jq '.result.logSyncHeight');     connectedPeers=$(echo $response | jq '.result.connectedPeers');     echo -e "logSyncHeight: \033[32m$logSyncHeight\033[0m, connectedPeers: \033[34m$connectedPeers\033[0m";     sleep 5; done
```

# Stop & Delete the service

```
sudo systemctl stop zgs
```

```
sudo systemctl disable zgs
sudo rm /etc/systemd/system/zgs.service
rm -rf $HOME/0g-storage-node
```

# Explorer & Useful Webs

* Explorer- (View your txs - Paste Your Address):
*  https://chainscan-galileo.bangcode.id/
*   OR
*   https://chainscan-galileo.0g.ai/

* View Miner Details- (Add your wallet address at the end of the link): https://storagescan-galileo.0g.ai/miner/

👉 Join TG for more Updates: https://t.me/ntekearning2
