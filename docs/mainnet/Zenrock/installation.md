# 🚀 Zenrock Installation Guide

This guide helps you set up and run a Zenrock Node.  
🔗 [Website](https://zenrocklabs.io/) | [Discord](https://discord.gg/hX4jng2s) | [Twitter](https://x.com/OfficialZenRock)

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
wget -O zenrockd.zip https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.25.0/zenrockd.zip
unzip zenrockd.zip
rm zenrockd.zip
chmod +x $HOME/zenrockd
sudo mv $HOME/zenrockd $HOME/go/bin/
```

---

## Init & Config
```bash
zenrockd init $MONIKER --chain-id diamond-1
zenrockd config set client chain-id diamond-1
zenrockd config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O $HOME/.zrchain/config/genesis.json https://github.com/kyronode/all-about-cosmos/raw/refs/heads/main/Mainnet/Zenrock/genesis.json
wget -O $HOME/.zrchain/config/addrbook.json https://github.com/kyronode/all-about-cosmos/raw/refs/heads/main/Mainnet/Zenrock/addrbook.json
```

---

## Set Seeds & Peers
```bash
SEEDS="e6c3373d68c504bd89bf77c27a8ac30597afeb2d@zenrock-mainnet-seed.itrocket.net:56656"
PEERS="2f037a6461c012f3296ab1815b3c47843bcd7c3a@zenrock-mainnet-peer.itrocket.net:59656,5ad8a5de6318529994da817043b268ef617e37ba@34.251.37.55:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.zrchain/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.zrchain/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.zrchain/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.zrchain/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.zrchain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.zrchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.zrchain/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0urock"|g' $HOME/.zrchain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.zrchain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.zrchain/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/zenrockd.service > /dev/null <<EOF
[Unit]
Description=zenrockd Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.zrchain
ExecStart=$(which zenrockd) start --home $HOME/.zrchain
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
sudo systemctl enable zenrockd
sudo systemctl restart zenrockd && sudo journalctl -u zenrockd -fo cat
```

---

## Node Synchronize Checker
```bash
# Paste this to your terminal
bash <(curl -s https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Mainnet/Zenrock/zenrock-sync.sh)
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
zenrockd keys add $WALLET
```
`If u want to restore use this command`
```bash
zenrockd keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(bitbadgeschaind keys show $WALLET -a)
VALOPER_ADDRESS=$(bitbadgeschaind keys show $WALLET --bech val -a)
echo "export WALLET_ADDRESS="$WALLET_ADDRESS >> $HOME/.bash_profile
echo "export VALOPER_ADDRESS="$VALOPER_ADDRESS >> $HOME/.bash_profile
source $HOME/.bash_profile
```

---

## Create Validator
If ur node has been fully synchronized, then u can create ur validator:
`Please Change <YOUR_MONIKER><YOUR_IDENTITY><YOUR_WEBSITE><YOUR_DETAILS> with ur own.`
```bash
cd $HOME
echo "{
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(zenrockd tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
  \"amount\": \"1000000urock\",
  \"moniker\": \"<YOUR_MONIKER>\",
  \"identity\": \"<YOUR_IDENTITY>\",
  \"website\": \"<YOUR_WEBSITE>\",
  \"security\": \"\",
  \"details\": \"<YOUR_DETAILS>\",
  \"commission-rate\": \"0.05\",
  \"commission-max-rate\": \"0.25\",
  \"commission-max-change-rate\": \"0.01\",
  \"min-self-delegation\": \"1\"
}" > validator.json

# Create a validator using the JSON configuration
zenrockd tx staking create-validator validator.json \
--from $WALLET \
--chain-id diamond-1 \
--gas auto \
--gas-adjustment 1.5 \
--fees 30urock \
-y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop zenrockd
sudo systemctl disable zenrockd
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/zenrockd.service
sudo rm $(which zenrockd)
sudo rm -rf $HOME/.zrchain
```