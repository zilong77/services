# 🚀 Node Installation Guide

This guide helps you set up and run a Lumera Node.  
🔗 [Website](https://lumera.io/) | [Discord](https://discord.gg/GUzSvAhF) | [Twitter](https://x.com/lumeraprotocol)

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
VER="1.23.4"
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
wget https://github.com/LumeraProtocol/lumera/releases/download/v1.7.0/lumera_v1.7.0_linux_amd64.tar.gz
tar -xvf lumera_v1.7.0_linux_amd64.tar.gz
rm lumera_v1.7.0_linux_amd64.tar.gz
rm install.sh
chmod +x lumerad
mv lumerad $HOME/go/bin/
mv libwasmvm.x86_64.so /usr/lib/
```

---

## Init & Config
```bash
lumerad init $MONIKER --chain-id lumera-testnet-2
lumerad config set client chain-id lumera-testnet-2
lumerad config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O $HOME/.lumera/config/genesis.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Lumera/genesis.json
wget -O $HOME/.lumera/config/addrbook.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Lumera/addrbook.json
```

---

## Set Seeds & Peers
```bash
SEEDS="408ede05d42c077c7e6f069e9dede07074f40911@94.130.143.184:19656"
PEERS="5b6aa55124c0fd28e47d7da091a69973964a9fe1@uno.sentry.testnet.v3.kiivalidator.com:26656,5e6b283c8879e8d1b0866bda20949f9886aff967@dos.sentry.testnet.v3.kiivalidator.com:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.lumera/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.lumera/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.lumera/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.lumera/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.lumera/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.lumera/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.lumera/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.025ulume"|g' $HOME/.lumera/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.lumera/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.lumera/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/lumerad.service > /dev/null <<EOF
[Unit]
Description=lumerad Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.lumera
ExecStart=$(which lumerad) start --home $HOME/.lumera
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
sudo systemctl enable lumerad
sudo systemctl restart lumerad && sudo journalctl -u lumerad -fo cat
```

---

## Node Synchronize Checker
```bash
# Paste this to your terminal
bash <(curl -s https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Testnet/Lumera/lumera-sync.sh)
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
lumerad keys add $WALLET
```
`If u want to restore use this command`
```bash
lumerad keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(lumerad keys show $WALLET -a)
VALOPER_ADDRESS=$(lumerad keys show $WALLET --bech val -a)
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
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(lumerad tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
  \"amount\": \"1000000ulume\",
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
lumerad tx staking create-validator validator.json \
    --from wallet \
    --chain-id lumera-testnet-2 \
    --fees 0.025ulume \
    --gas-adjustment 1.3 \
    --gas auto \
    -y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop lumerad
sudo systemctl disable lumerad
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/lumerad.service
sudo rm $(which lumerad)
sudo rm -rf $HOME/.lumera
```