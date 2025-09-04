# 🚀 Paxi Installation Guide

This guide helps you set up and run a Bitbadges Node.  
🔗 [Website](https://paxinet.io/) | [Discord](https://discord.gg/rA9Xzs69tx) | [Twitter](https://x.com/paxiweb3)

---

## 📋 Recommended Specifications

| Component | Minimum | Recommended |
|-----------|----------|-------------|
| CPU       | 4 Cores  | 4+ Cores    |
| RAM       | 8 GB     | 8+ GB       |
| Storage   | 400 GB SSD | 1 TB NVMe SSD |
| OS        | Ubuntu 22.04 | Ubuntu 22.04 |

---

## Install Go (if you don't have one)
```bash
cd $HOME
VER="1.24.4"
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
git clone https://github.com/paxi-web3/paxi.git
cd paxi
git checkout v1.0.5
make install
cp ~/paxid/paxid ~/go/bin
```

---

## Init & Config
```bash
paxid init $MONIKER --chain-id paxi-mainnet
paxid config set client chain-id paxi-mainnet
paxid config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
curl -Ls https://snapshot-t.vinjan.xyz/paxi/genesis.json > ~/go/bin/paxi/config/genesis.json
curl -Ls https://snapshot-t.vinjan.xyz/paxi/addrbook.json > ~/go/bin/paxi/config/addrbook.json
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
s%:6065%:${APP_PORT}065%g" ~/go/bin/paxi/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" ~/go/bin/paxi/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" ~/go/bin/paxi/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" ~/go/bin/paxi/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" ~/go/bin/paxi/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.01upaxi"|g' ~/go/bin/paxi/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" ~/go/bin/paxi/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" ~/go/bin/paxi/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/paxid.service > /dev/null <<EOF
[Unit]
Description=paxi
After=network-online.target
[Service]
User=$USER
ExecStart=$(which paxid) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

---

## Start Service
```bash
sudo systemctl daemon-reload
sudo systemctl enable paxid
sudo systemctl restart paxid && sudo journalctl -u paxid -fo cat
```

---

## Install Wasm Sync
```bash
curl -sL https://raw.githubusercontent.com/vinjan23/Mainnet/refs/heads/main/Paxi/wasm |bash
sudo systemctl restart paxid
```

---

## Snapshot
```bash
sudo apt install lz4 -y
sudo systemctl stop paxid
cp ~/go/bin/paxi/data/priv_validator_state.json ~/go/bin/paxi/priv_validator_state.json.backup
paxid tendermint unsafe-reset-all --home ~/go/bin/paxi --keep-addr-book
curl -L https://snapshot-t.vinjan.xyz/paxi/latest.tar.lz4  | lz4 -dc - | tar -xf - -C ~/go/bin/paxi
mv ~/go/bin/paxi/priv_validator_state.json.backup ~/go/bin/paxi/data/priv_validator_state.json
sudo systemctl restart paxid && sudo journalctl -u paxid -fo cat
```

---

## Node Synchronize Checker
```bash
#!/bin/bash
rpc_port=$(grep -m 1 -oP '^laddr = "\K[^"]+' "~/go/bin/paxi/config/config.toml" | cut -d ':' -f 3)
while true; do
  local_height=$(curl -s localhost:$rpc_port/status | jq -r '.result.sync_info.latest_block_height')
  network_height=$(curl -s https://mainnet-rpc.paxinet.io/status | jq -r '.result.sync_info.latest_block_height')
  if ! [[ "$local_height" =~ ^[0-9]+$ ]] || ! [[ "$network_height" =~ ^[0-9]+$ ]]; then
    echo -e "\033[1;31mError: Invalid block height data. Retrying...\033[0m"
    sleep 5
    continue
  fi
  blocks_left=$((network_height - local_height))
  echo -e "\033[1;33mNode Height:\033[1;34m $local_height\033[0m \
\033[1;33m| Network Height:\033[1;36m $network_height\033[0m \
\033[1;33m| Blocks Left:\033[1;31m $blocks_left\033[0m"
  sleep 5
done
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
paxid keys add $WALLET
```
`If u want to restore use this command`
```bash
paxid keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(paxid keys show $WALLET -a)
VALOPER_ADDRESS=$(paxid keys show $WALLET --bech val -a)
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
  \"pubkey\": {\"@type\":\"/cosmos.crypto.ed25519.PubKey\",\"key\": \"$(paxid tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
  \"amount\": \"1000000upaxi\",
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
paxid tx staking create-validator validator.json \
    --from wallet \
    --chain-id paxi-mainnet \
    --fees 15000upaxi \
    --gas-adjustment 1.3 \
    --gas auto \
    -y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop paxid
sudo systemctl disable paxid
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/paxid.service
sudo rm $(which paxid)
sudo rm -rf ~/go/bin/paxi/
```


Thanks to: [Vinjan.Inc](https://service.vinjan.xyz)