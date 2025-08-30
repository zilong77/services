# 🚀 Node Installation Guide

This guide helps you set up and run a Kiichain Node.  
🔗 [Website](https://www.kiiglobal.io/) | [Discord](https://discord.gg/4wNJRqR9) | [Twitter](https://x.com/KiiChainio)

---

## 📋 Recommended Specifications

| Component | Recommended |
|-----------|----------|
| CPU       | 4 Cores  |
| RAM       | 8 GB     | 
| Storage   | 1 TB NVMe SSD |
| OS        | Ubuntu 22.04 |

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
rm -rf safrochain-node
git clone https://github.com/Safrochain-Org/safrochain-node.git
cd safrochain-node
git checkout v0.1.0
make install
```

---

## Init & Config
```bash
safrochaind init $MONIKER --chain-id oro_1336-1
safrochaind config set client chain-id oro_1336-1
safrochaind config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O $HOME/.safrochain/config/genesis.json https://github.com/kyronode/all-about-cosmos/raw/refs/heads/main/Testnet/Safrochain/genesis.json
wget -O $HOME/.safrochain/config/addrbook.json https://github.com/kyronode/all-about-cosmos/raw/refs/heads/main/Testnet/Safrochain/addrbook.json
```

---

## Set Seeds & Peers
```bash
SEEDS="408ede05d42c077c7e6f069e9dede07074f40911@94.130.143.184:19656"
PEERS="5b6aa55124c0fd28e47d7da091a69973964a9fe1@uno.sentry.testnet.v3.kiivalidator.com:26656,5e6b283c8879e8d1b0866bda20949f9886aff967@dos.sentry.testnet.v3.kiivalidator.com:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.safrochain/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.safrochain/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.safrochain/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.safrochain/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.safrochain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.safrochain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.safrochain/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.001usaf"|g' $HOME/.safrochain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.safrochain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.safrochain/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/safrochaind.service > /dev/null <<EOF
[Unit]
Description=safrochaind Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.safrochain
ExecStart=$(which safrochaind) start --home $HOME/.safrochain
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
sudo systemctl enable safrochaind
sudo systemctl restart safrochaind && sudo journalctl -u safrochaind -fo cat
```

---

## Node Synchronize Checker
```bash
# Paste this to your terminal
bash <(curl -s https://github.com/kyronode/all-about-cosmos/raw/refs/heads/main/Testnet/Safrochain/safro-sync.sh)
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
safrochaind keys add $WALLET
```
`If u want to restore use this command`
```bash
safrochaind keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(safrochaind keys show $WALLET -a)
VALOPER_ADDRESS=$(safrochaind keys show $WALLET --bech val -a)
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
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(safrochaind tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
  \"amount\": \"1000000000000000000akii\",
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
safrochaind tx staking create-validator validator.json \
    --from wallet \
    --chain-id oro_1336-1 \
    --gas-prices 1250000000akii \
    --gas-adjustment 1.3 \
    --gas auto \
    -y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop safrochaind
sudo systemctl disable safrochaind
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/safrochaind.service
sudo rm $(which safrochaind)
sudo rm -rf $HOME/.safrochain
```