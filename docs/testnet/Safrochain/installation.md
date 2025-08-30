# 🚀 Safrochain Installation Guide

This guide helps you set up and run a Safrochain Node.  
🔗 [Website](https://safrochain.com/) | [Discord](https://discord.gg/3Q3w3mqvXU) | [Twitter](https://x.com/safrochain)

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
SEEDS="2242a526e7841e7e8a551aabc4614e6cd612e7fb@88.99.211.113:26656,642dfd491b8bfc0b842c71c01a12ee1122f3dafe@46.62.140.103:26656"
PEERS="9df9e898213296e0180bdad54c30cb5f3c424a42@78.46.215.249:26656,1ac9964262094380ab20278979de72280d7bfc59@152.53.182.15:21656,642dfd491b8bfc0b842c71c01a12ee1122f3dafe@46.62.140.103:26656,693c44c8fdeea31ceadf43f98d73fdf317bd70b8@62.169.16.57:21656,88fb1dd0ed4e81389215ce663a7219d0ce54cf59@161.97.101.168:26656,d001827cf6adb1b6b63284189127e5d844173889@143.198.91.87:26656,b4b711560e62b3a850193f3fa85c82e6ccf4c013@135.181.178.120:12656,525aed9c85f89d9a70c0f2048b1b0e7695c3d03a@37.60.252.195:21656,637077d431f618181597706810a65c826524fd74@176.9.120.85:32756"
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
  \"amount\": \"1000000usaf\",
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
    --from $WALLET \
    --chain-id safro-testnet-1 \
    --fees 5000usaf \
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