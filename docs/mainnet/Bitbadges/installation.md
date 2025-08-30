# 🚀 Bitbadges Installation Guide

This guide helps you set up and run a Bitbadges Node.  
🔗 [Website](https://bitbadges.io/) | [Discord](https://discord.gg/WN9XgGun) | [Twitter](https://x.com/bitbadges_io)

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
mkdir -p $HOME/.bitbadgeschain
wget https://github.com/BitBadges/bitbadgeschain/releases/download/v13/bitbadgeschain-linux-amd64 -O /usr/local/bin/bitbadgeschaind
chmod +x /usr/local/bin/bitbadgeschaind
```

---

## Init & Config
```bash
bitbadgeschaind init $MONIKER --chain-id bitbadges-1
bitbadgeschaind config set client chain-id bitbadges-1
bitbadgeschaind config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O $HOME/.bitbadgeschain/config/genesis.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Mainnet/Bitbadges/genesis.json
wget -O $HOME/.bitbadgeschain/config/addrbook.json https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Mainnet/Bitbadges/addrbook.json
```

---

## Set Seeds & Peers
```bash
SEEDS="a26f1e34a79ef25037bdcdbf675b5eb9b6f0ee48@135.181.79.101:11756"
PEERS="f23f1bbd14dc5d2bec36ac6179ec10646e6c441b@149.86.227.180:12056,01646d1b565922a8f9b29990d4f84418be277fdd@159.69.29.172:38656,2e5db65945f66f56c5c2c16b267936de902a28dc@152.53.254.219:13556,a9d17c23090a03b242ce110cccf517b04be99699@51.195.60.23:32956,5f0816cc8802e933f7894200a8effbcde8adaea2@65.108.204.225:32956,1be74320788c2b42bc6e0855ae0f77a80aa84b94@65.108.7.249:32956,1b96669cb6abab3c29b58e35f42dfa667d4d5763@157.180.6.152:24656,2df37851a4b2679dc3db0822318ec403d0e527bd@37.27.52.37:27656,98dac6b8801ce7aa956841b2b67d3c92c6e4374e@135.181.238.225:17156,6af383dd6b8cb9ab7075ef552bd0ba0361c09a6e@152.53.91.160:38656,f545cbb1a01c6846e17ea8785da5d5c7042da009@65.21.132.31:22656,1c80babccceb5cb4ed40959017348561f9170f66@46.232.249.218:13556,90a0e3bb00b1bc8c9b47bea15b9c850d1cf07c69@38.242.134.158:38656,a26f1e34a79ef25037bdcdbf675b5eb9b6f0ee48@135.181.79.101:11756,18b9107afecfc97797b7c0237ed48f35543f2753@152.53.147.91:26656,01c6914b236aa686e6ad0daa88affcbf6802dd7b@37.120.178.250:13556,13bf086c3e777f5b24d51c1fe322182dc3321f36@157.245.156.73:10656,ddac35eb1f99c15f3caa26febf2e19e2a937ed4d@152.53.182.15:13556,f1aaac0ff6cb795ea254bfb2c4df6db185539776@149.102.143.185:38656,dcf245b19e2089b7006b21a273eeb9833f55b774@159.223.85.249:13556,47b6fde031c1513f2a8887147c48c7bdbcf96791@65.21.234.111:13556,dcfe30be9cb585d1f8391ae06cc71847f6ef85e4@84.247.185.120:13556,afa28c23cb8ccea8a6f6474bc1359412c68751bc@173.212.196.38:17956,d0605b76f3618e488db0ee417258f6296c078bf1@149.50.101.137:12256,9d8affecc081f6d9fa48cc889a21fc7e2865bde6@152.53.162.92:13556,4d4021ca298aaa65d39fe74b4b684160282094d9@164.68.111.174:17956,67e1d00378d985bb04c48faae25aba3b44f2e04b@152.53.255.227:13556,03e65d19321f384207c5a923627176453655c911@65.108.198.145:21656,02190b59f894324cbcb390072b8a443ddcd72626@135.181.139.249:36656,503bcca2e177810d388044bb3cbb628bd1588b3c@152.53.250.230:13556,67ed37309251eea4f0884adcc2517b0dad7ea52e@157.180.52.245:20656,a2b7feb3a3646d7c5b7f1bed1068cc77ce188c36@152.53.147.137:13556,e65ae8c8fbf16d1f49f072e33963c3cb59877e3e@88.99.149.170:13556,327fb4151de9f78f29ff10714085e347a4e3c836@82.27.2.107:666"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.bitbadgeschain/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.bitbadgeschain/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.bitbadgeschain/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.bitbadgeschain/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.bitbadgeschain/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.bitbadgeschain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.bitbadgeschain/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.025ubadge"|g' $HOME/.bitbadgeschain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.bitbadgeschain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.bitbadgeschain/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/bitbadgeschaind.service > /dev/null <<EOF
[Unit]
Description=bitbadgeschaind Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.bitbadgeschain
ExecStart=$(which bitbadgeschaind) start --home $HOME/.bitbadgeschain
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
sudo systemctl enable bitbadgeschaind
sudo systemctl restart bitbadgeschaind && sudo journalctl -u bitbadgeschaind -fo cat
```

---

## Node Synchronize Checker
```bash
# Paste this to your terminal
bash <(curl -s https://raw.githubusercontent.com/kyronode/all-about-cosmos/refs/heads/main/Mainnet/Bitbadges/bitbadges-sync.sh)
```

---

## Create or Restore Wallet
`Create new wallet and save ur mnemonics securely`
```bash
bitbadgeschaind keys add $WALLET
```
`If u want to restore use this command`
```bash
bitbadgeschaind keys add $WALLET --recover
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
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(bitbadgeschaind tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
  \"amount\": \"1000000000ubadge\",
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
bitbadgeschaind tx staking create-validator validator.json \
    --from wallet \
    --chain-id bitbadges-1 \
    --gas-prices 0.025ubadge \
    --gas-adjustment 1.3 \
    --gas auto \
    -y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop bitbadgeschaind
sudo systemctl disable bitbadgeschaind
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/bitbadgeschaind.service
sudo rm $(which bitbadgeschaind)
sudo rm -rf $HOME/.bitbadgeschain
```