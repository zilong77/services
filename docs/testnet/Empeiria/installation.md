# 🚀 Empeiria Installation Guide

This guide helps you set up and run a Empeiria Node.  
🔗 [Website](https://empe.io/) | [Discord](https://discord.com/invite/BbCzfuJzkv) | [Twitter](https://x.com/empe_io)

---

## 📋 Recommended Specifications

| Component | Recommended |
|-----------|----------|
| CPU       | 4 Cores  |
| RAM       | 8 GB     |
| Storage   | 200 GB NVMe SSD |
| OS        | Ubuntu 22.04 |

---

## Install Go (if you don't have one)
```bash
cd $HOME
VER="1.23.1"
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
# WARNING: Please replace <YOUR_WALLET>, <YOUR_MONIKER>, and <CUSTOM_PORT>
# with your own information before running the commands.
echo "export WALLET=<YOUR_WALLET>" >> $HOME/.bash_profile
echo "export MONIKER=<YOUR_MONIKER>" >> $HOME/.bash_profile
echo "export APP_PORT=<CUSTOM_PORT>" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

---

## Download Binary
```bash
cd $HOME
rm -rf bin
mkdir bin
cd $HOME/bin
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.4.0/emped_v0.4.0_linux_amd64.tar.gz
tar -xvf emped_v0.4.0_linux_amd64.tar.gz
chmod +x $HOME/bin/emped
mv $HOME/bin/emped ~/go/bin
```

---

## Init & Config
```bash
emped config node tcp://localhost:${APP_PORT}657
emped config keyring-backend os
emped config chain-id empe-testnet-2
emped init "test" --chain-id empe-testnet-2
```

---

## Download Genesis & Addrbook
```bash
wget -O $HOME/.empe-chain/config/genesis.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/raw/refs/heads/main/Testnet/Empe/genesis.json
wget -O $HOME/.empe-chain/config/addrbook.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/raw/refs/heads/main/Testnet/Empe/addrbook.json
```

---

## Set Seeds & Peers
```bash
SEEDS="d3b593104147a4c7d0f59f3221e1f68d1f6a759b@135.181.79.101:42656"
PEERS="5677d8f7aeded49628f536752db2d5aeae44ec9f@89.250.74.164:24656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.empe-chain/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.empe-chain/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.empe-chain/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.empe-chain/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.empe-chain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.empe-chain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.empe-chain/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.0001uempe"|g' $HOME/.empe-chain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.empe-chain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.empe-chain/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/emped.service > /dev/null <<EOF
[Unit]
Description=emped Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.empe-chain
ExecStart=$(which emped) start --home $HOME/.empe-chain
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
sudo systemctl enable emped
sudo systemctl restart emped && sudo journalctl -u emped -fo cat
```

---

## Node Synchronize Checker
```bash
# Paste this to your terminal
bash <(curl -s https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Empe/empe-sync.sh)
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
emped keys add $WALLET
```
`If u want to restore use this command`
```bash
emped keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(emped keys show $WALLET -a)
VALOPER_ADDRESS=$(emped keys show $WALLET --bech val -a)
echo "export WALLET_ADDRESS="$WALLET_ADDRESS >> $HOME/.bash_profile
echo "export VALOPER_ADDRESS="$VALOPER_ADDRESS >> $HOME/.bash_profile
source $HOME/.bash_profile
```

---

## Create Validator
If ur node has been fully synchronized, then u can create ur validator:
`Please Change <YOUR_MONIKER><YOUR_IDENTITY><YOUR_WEBSITE><YOUR_DETAILS> with ur own.`
```bash
emped tx staking create-validator \
--amount 1000000uempe \
--from $WALLET \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(emped tendermint show-validator) \
--moniker "<YOUR_MONIKER>" \
--identity "<YOUR_IDENTITY>" \
--website "<YOUR_WEBSITE>" \
--details "<YOUR_DETAILS>" \
--chain-id empe-testnet-2 \
--gas auto --gas-adjustment 1.5 --fees 30uempe \
-y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop emped
sudo systemctl disable emped
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/emped.service
sudo rm $(which emped)
sudo rm -rf $HOME/.empe-chain
```