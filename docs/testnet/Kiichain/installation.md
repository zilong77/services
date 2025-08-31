# 🚀 Node Installation Guide

This guide helps you set up and run a Kiichain Node.  
🔗 [Website](https://www.kiiglobal.io/) | [Discord](https://discord.gg/4wNJRqR9) | [Twitter](https://x.com/KiiChainio)

---

## 📋 Recommended Specifications

| Component | Minimum | Recommended |
|-----------|----------|-------------|
| CPU       | 4 Cores  | 4+ Cores    |
| RAM       | 8 GB     | 8+ GB       |
| Storage   | 1 TB NVMe SSD | 2 TB NVMe SSD |
| OS        | Ubuntu 22.04 | Ubuntu 22.04 |

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
rm -rf kiichain
git clone https://github.com/KiiChain/kiichain.git
cd kiichain
git checkout v4.0.0
make install
```

---

## Init & Config
```bash
kiichaind init $MONIKER --chain-id oro_1336-1
kiichaind config set client chain-id oro_1336-1
kiichaind config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O $HOME/.kiichain/config/genesis.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Kiichain/genesis.json
wget -O $HOME/.kiichain/config/addrbook.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Kiichain/addrbook.json
```

---

## Set Seeds & Peers
```bash
SEEDS="408ede05d42c077c7e6f069e9dede07074f40911@94.130.143.184:19656"
PEERS="5b6aa55124c0fd28e47d7da091a69973964a9fe1@uno.sentry.testnet.v3.kiivalidator.com:26656,5e6b283c8879e8d1b0866bda20949f9886aff967@dos.sentry.testnet.v3.kiivalidator.com:26656,37efd4fe6fe3d7a02f5fd3be50d87c40c22c538e@3.21.163.223:26656,5586eeacaf24dc9e1ac7114712c59b9a4008cb17@157.180.3.36:26656,b44c9a8f83e6e54398ebe0071291f4910e2ac06d@195.26.251.0:19656,0ed98f1fbd521061dd2aebf5006e0210a3fdc514@38.242.201.82:26656,49f6ec41d2f9fb80bef1fc94f68401327b18bc5b@144.217.68.182:29956,4acc0d6cd7d986ddcc285ca84f0751e72a3551e1@62.169.25.150:26656,060580ec077b57ff689b7f7ff99cb11a2b7bebaf@167.86.119.59:26656,0d2f5687a9aad73e8452053509e95f15f8e7cb2d@152.53.244.37:17656,5d56f35169f9b18feccc02065c5ab3fd00aadd89@88.99.137.138:31656,ddff96cc52a1977bac5cfa9ecaa24b4f7f4a91b0@156.67.29.226:26656,4fa1d8a96e7b1daa508e4e54aceaef6a5fbae2b5@144.91.105.79:26656,b0f5496acf5274e15f0c1480a3a5b42e24f26931@46.232.249.218:26656,75077b7012727fd0d9a46f384d4d2b8905b40a9e@152.53.228.204:26656,5faaa7bafc2c75ad7fc15734d28e172e983e324c@157.180.52.245:15656,8196802a14a6c79e0689e39bec66f3f7e5779c6c@172.81.133.221:26656,df675aa09d8993a7a24c33d6937efa4c77b8569c@23.129.20.122:31556,98695c993641bc3193661cae67597d35caf60194@152.53.84.190:26656,89137ae052578557a6062557cc8c6cce6b08ab13@65.108.198.145:62656,408ede05d42c077c7e6f069e9dede07074f40911@94.130.143.184:19656,dfd5615a230f2876836757a7168a950d7dfc7a8e@149.50.116.116:29656,169a84de3aa039c6258643fc11a973e261e87d2a@135.125.97.162:31556,6c7fb62f1583ae3b9f87409455dc0bbf73f151e5@138.199.223.113:26656,74a8afe2428f3488b96a52ea0d5859bf608ea18a@135.181.178.120:31656,7699784d63c212b22cb552cd244f50df22fbcf42@5.75.167.51:26656,3e0cd23706b949076d7b3c629cc49e4ef1a32d44@23.129.20.121:31556"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.kiichain/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.kiichain/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.kiichain/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.kiichain/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.kiichain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.kiichain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.kiichain/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "1000000000akii"|g' $HOME/.kiichain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.kiichain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.kiichain/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/kiichaind.service > /dev/null <<EOF
[Unit]
Description=kiichaind Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.kiichain
ExecStart=$(which kiichaind) start --home $HOME/.kiichain
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
sudo systemctl enable kiichaind
sudo systemctl restart kiichaind && sudo journalctl -u kiichaind -fo cat
```

---

## Node Synchronize Checker
```bash
# Paste this to your terminal
bash <(curl -s https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Kiichain/kiichain-sync.sh)
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
kiichaind keys add $WALLET
```
`If u want to restore use this command`
```bash
kiichaind keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(kiichaind keys show $WALLET -a)
VALOPER_ADDRESS=$(kiichaind keys show $WALLET --bech val -a)
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
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(kiichaind tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
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
kiichaind tx staking create-validator validator.json \
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
sudo systemctl stop kiichaind
sudo systemctl disable kiichaind
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/kiichaind.service
sudo rm $(which kiichaind)
sudo rm -rf $HOME/.kiichain
```