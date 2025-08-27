# 🚀 Node Installation Guide

This guide helps you set up and run a **Node** for the project.  
🔗 [Website](https://official-website.com) | [Discord](https://discord.gg/project) | [Twitter](https://twitter.com/project)

---

## 📋 Recommended Specifications

| Component | Minimum | Recommended |
|-----------|----------|-------------|
| CPU       | 2 Cores  | 4+ Cores    |
| RAM       | 4 GB     | 8+ GB       |
| Storage   | 80 GB SSD| 200+ GB NVMe|
| OS        | Ubuntu 20.04+ | Ubuntu 22.04 |

---

## Install Go (if you don't have one)
```bash
cd $HOME
VER="1.23.8"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"

[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

---

## Set Environment Variables
```bash
echo "export WALLET=<YOUR_WALLET>" >> $HOME/.bash_profile
echo "export MONIKER=<YOUR_MONIKER>" >> $HOME/.bash_profile
echo "export APP_CHAIN_ID=<CHAIN_ID>" >> $HOME/.bash_profile
echo "export APP_PORT=<CUSTOM_PORT>" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

---

## Download Binary
```bash
cd $HOME
wget -O $APPD.zip <BINARY_URL>
unzip $APPD.zip
rm $APPD.zip
chmod +x $HOME/$APPD
sudo mv $HOME/$APPD $HOME/go/bin/
```

---

## Init & Config
```bash
$APPD init $MONIKER --chain-id $APP_CHAIN_ID
$APPD config set client chain-id $APP_CHAIN_ID
$APPD config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O $APP_HOME/config/genesis.json <GENESIS_URL>
wget -O $APP_HOME/config/addrbook.json <ADDRBOOK_URL>
```

---

## Set Seeds & Peers
```bash
SEEDS="<SEED_NODE>"
PEERS="<PEER_NODE1>,<PEER_NODE2>"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $APP_HOME/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $APP_HOME/config/config.toml
```

---

## Custom Ports
```bash
# app.toml
sed -i.bak -e "s%:1317%:${APP_PORT}317%g;
s%:8080%:${APP_PORT}080%g;
s%:9090%:${APP_PORT}090%g;
s%:9091%:${APP_PORT}091%g;
s%:8545%:${APP_PORT}545%g;
s%:8546%:${APP_PORT}546%g;
s%:6065%:${APP_PORT}065%g" $APP_HOME/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $APP_HOME/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $APP_HOME/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $APP_HOME/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $APP_HOME/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "<MIN_GAS_PRICE>"|g' $APP_HOME/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $APP_HOME/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $APP_HOME/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/$APPD.service > /dev/null <<EOF
[Unit]
Description=$APPD Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$APP_HOME
ExecStart=$(which $APPD) start --home $APP_HOME
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

---

## Start Service
```bash
sudo systemctl daemon-reload
sudo systemctl enable $APPD
sudo systemctl restart $APPD && sudo journalctl -u $APPD -fo cat
```
