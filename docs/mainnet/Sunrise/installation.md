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
git clone https://github.com/sunriselayer/sunrise.git
cd sunrise
git checkout v1.1.0-make-install
make install
```

---

## Init & Config
```bash
sunrised init $MONIKER --chain-id sunrise-1
sunrised config set client chain-id sunrise-1
sunrised config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O genesis.json https://snapshots.polkachu.com/genesis/sunrise/genesis.json --inet4-only
mv genesis.json ~/.sunrise/config
wget -O addrbook.json https://snapshots.polkachu.com/addrbook/sunrise/addrbook.json --inet4-only
mv addrbook.json ~/.sunrise/config

```

---

## Set Seeds & Peers
```bash
SEEDS="5bc75c4efd94656d5f6e36d0f742bb2b3fef5f35@sunrise-seed.noders.services:36156"
PEERS="3b02bfd265b4b67d867c0da47f085dd626100569@65.21.132.31:21656,13c1a5edd2e09c8ec3998fdc2c8ede330c6224bf@65.108.204.225:28356,34e39405f02872a4a9403f241066cf0875a66ce2@65.108.7.249:28356,9e3fdd30b550024f09df85d6e7f9a3c3daa32b36@34.87.34.133:26656,401d10017915a9f217cb7e9ae9c888556c81e6da@65.108.230.75:18656,bb69adc6246d31899055c2da852ef5c3fd5bbfe3@51.195.60.23:28356,12b53dc2769314124f7b2cad15645032a0d06417@188.245.248.134:26656,13b3e44f5164b7e85871e9b58f262ca1097b1653@158.69.125.73:11556,6321f379be8a43731c15726c979e4993de433e14@65.21.130.53:26656,2494005ba072167d29e6f55a9b378a781872a49e@65.109.145.247:26656,fc93745fb6d05955f5d3e107d495c69d7d545828@91.99.196.242:26656,41caa4106f68977e3a5123e56f57934a2d34a1c1@185.16.38.210:27566,da1b772776bcc3c84e21d7aa72314f58c2729a43@65.109.18.169:36156,e776df4c573785a3416da430fb9c90be72ea795e@23.129.20.120:28356,41e61c0441f0086caf17186950fc440d45870bf5@198.13.54.102:26656,1d59382e2a6db1f80ca90b7cf1df1fcada237fab@65.108.68.53:26670,d15cdf5150c5e761c872982193a784a540fefe9d@148.251.66.35:40656,366ef5ffeb16a2d8f018f483bf163bf75563d556@94.130.35.120:19656,8674ccae07a38d310afda5104273db2e1ece5e92@188.40.102.137:30656,9f8312a2f9cf96017d17acc55216bd813111a6ed@45.250.252.75:56326,97ae5714afec408cb6f5fb78cf830af8c5d7b535@65.109.104.17:26656,a54886fba0cf118825d616e0caf9d52c534d7042@82.22.184.97:26656,60a83f51c20d39b7e69594f538513a80521eb0e8@45.32.30.169:26656,8b13908c44911f9797798e951557c17ef2490ee8@95.217.203.185:26656,8b4136343917f704c81c8e49618a41472d8e1abe@65.108.229.19:26776,34447c658f69fa1bc56125e991c207da1efbf137@65.109.59.22:28356,49be16d94c586f3ebd15f7cc7174d56765043b11@64.185.226.202:28356,7db7f656d36c420f39a8eab76c50c41cff440fa9@65.109.58.158:28356,2a30964e07118a0cb03bb1cae9185d37a967230a@207.148.68.35:26656,36fc07d6ab5ab85349cb563bf033214eae9973a2@148.251.43.226:25656,141a8536c48d5cab50439ef627951cc7e4e311bf@65.108.198.145:22656,2404dca4d4b0831e69dd010539f0c391bcd0523a@95.217.128.50:26656,6af5937e4180e3132906439b6eebea6dff017c9e@157.180.6.152:22656,345a34c3e1b6c82af4e85fafa03d27980332dac7@37.252.186.241:26656,da0f328712a72cff3e472c43ae3fa4020f9d420b@52.68.80.18:26656,2d712853b8aeca55161a71f1f5ca8bb27cc499d2@38.146.3.231:28356,00e7f72cf8a21bb381b8810b27cccaa00a51cf96@113.43.234.98:26666,c27e826d43b92c045cf2c1b9863154772ed68c04@65.21.233.188:28356,50c3a76591c4827ef97968a742f9450b19e474b6@128.140.68.218:26656,c2450c69db7413dd7d8670a43c20269e8e2dc16e@50.46.175.241:28356,095dcea05d385dd951999a0f57c75e9cbf378e77@5.9.69.107:36156,211e3de05bec1deb2d641a4855968f02044379eb@23.88.67.245:46656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.sunrise/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.sunrise/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.sunrise/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.sunrise/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.sunrise/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.sunrise/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.sunrise/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0urise"|g' $HOME/.sunrise/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.sunrise/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.sunrise/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/sunrised.service > /dev/null <<EOF
[Unit]
Description=sunrised Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.sunrise
ExecStart=$(which sunrised) start --home $HOME/.sunrise
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
sudo systemctl enable sunrised
sudo systemctl restart sunrised && sudo journalctl -u sunrised -fo cat
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
sunrised keys add $WALLET
```
`If u want to restore use this command`
```bash
sunrised keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(sunrised keys show $WALLET -a)
VALOPER_ADDRESS=$(sunrised keys show $WALLET --bech val -a)
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
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(sunrised tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
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
sunrised tx staking create-validator validator.json \
--from $WALLET \
--chain-id sunrise-1 \
--gas auto \
--gas-adjustment 1.5 \
--fees 30urock \
-y
```

---

## Delete Node
`Please backup ur wallet and important file like "priv_validator_key.json" before deleting.` 
```bash
sudo systemctl stop sunrised
sudo systemctl disable sunrised
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/sunrised.service
sudo rm $(which sunrised)
sudo rm -rf $HOME/.sunrise
```