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
uniond init $MONIKER --chain-id union-1
uniond config set client chain-id union-1
uniond config set client node tcp://localhost:${APP_PORT}657
```

---

## Download Genesis & Addrbook
```bash
wget -O genesis.json https://snapshots.polkachu.com/genesis/union/genesis.json --inet4-only
mv genesis.json ~/.union/config
wget -O addrbook.json https://snapshots.polkachu.com/addrbook/union/addrbook.json --inet4-only
mv addrbook.json ~/.union/config

```

---

## Set Seeds & Peers
```bash
SEEDS="ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:24656"
PEERS="3ad5669d506563c2bc61694f71906828be437be0@65.21.10.181:24656,14cdfe6797eabe850201da2b2ff67714a08f30ac@65.109.92.37:25656,da60523597a0700e0f82a6deac57a35eaac40bce@195.14.6.190:26656,411be35332eb307887e5ad85696846b869d7269d@65.108.204.209:22656,bf6f4603748b6f0bde0ac23524aca14a5a95dde2@135.181.239.99:24656,25558ce3247e3e216f8cb41fb8de819b5fa8d8e1@88.216.198.174:18700,88552d615aad85723957f65f63083430414393d9@5.78.114.31:26656,8c4b4b26a5d05479561175d456d866f4ca9bfcd5@15.235.228.165:24656,0e89105a11dcdca92a4d5a13221d4604d6e80b62@167.235.184.49:36656,af49d79474eb3010c948264ec54d80eaca232465@15.204.44.87:26656,5fd464a7f32b1a1855b96b941e1d3e310092e505@65.108.134.47:22656,41caa4106f68977e3a5123e56f57934a2d34a1c1@195.3.221.10:27026,a4b20fefab6c50663abe255204d0da6e95b456e9@91.98.30.107:26656,6ed816f673b15324271362c0533b4aec95a35f96@94.130.35.120:25656,558066a9bf685f18c333d9c5c01d62004820c982@157.180.8.121:39656,f49f04e329313e01b8b32f9bdbd3aeff8a10774c@190.2.142.45:26656,d1bbe6960983a637b0b2cc615c341b22e596b609@95.216.243.91:23656,1d7f55b876167eea4cfaea103c61834c63a6cee2@5.9.99.42:24656,27844028beb392062b00802b15af1b289ece2f10@78.46.90.139:26656,5929dd8592ef5ad21c5773326e5bf9f6d0d2e7d6@149.248.38.121:26656,3620a928a4b196c0c5e55dc06303e7a959daccdb@193.34.213.191:10007,2d318979f882046acea037038212ec1995db6390@65.109.58.158:24656,8e546185b6faccd7c10ad6b6cb7a5cbb72560099@15.235.230.236:24656,16953d1e4291aa8429cef62581510ac6084b4bdd@160.250.106.109:22656,1b9e6e2897283783f4f7a357bb95ccc92eedc757@65.109.26.242:24656,b5082cb8c99b702492c4737dde68e6bbf55c01b7@188.40.85.190:14456,b5e8da8636b03609eed521b1e569878f3a42c36e@213.45.124.230:26656,c7368f16bdd2c12b4edeb862b85abd28e6f61e84@46.4.64.123:26656,be75c1d9c320bd649317fd7146de3d925435fd40@88.99.51.91:22656,c998dbe3250e3b34edf7feaeacdc857b13cafd78@65.108.238.102:24656,ac5f7eb81cd2b1d5b70d34e148842b917c33f7f6@5.78.132.118:26656,f8971b3cbed5f87858523c249b6ad664d6a416c4@152.53.236.93:26656,4dea424cc4fe56441601f8a28cbaa8f346631ccf@159.89.4.95:31530,684c91e2861c3ccf5c9feab6b51131e58b40b2f1@51.159.96.236:46656,0316dad6507f63aaf596d725db87bbf961325cd0@51.91.116.146:46656,374c74639cf16588ce6bba89cc9649beea5a8925@213.239.221.157:26656,a34d051d23d9ae46ce4af7eb0a084e65e490bb71@51.161.172.54:56076,ccef1cfc10e21c211677230bb5fda48766395e92@50.46.175.241:24656,21dba05c923020346a6e2ea9e3497b76e9653c43@193.34.212.38:44656,3ebf31d59814eb8edbbf3f9afffe7854b89725cf@148.251.245.34:14456,2b9a5eb9dd3c2ba34771b72c6791d6c18b4ee8a5@91.99.182.233:26656,45e9650c107d712a749579df27a0fdebc54003bb@65.108.205.121:24656,e18a68cddd88701eb38600969fc3ae180a3f7daf@65.108.232.168:18656,0968495362c138b46b47bb653da8cff01ffc2a84@213.170.133.18:26656,15e31ed4eeccec00266f8671a6a58b90d810aafc@185.26.11.73:56076,e4599579642d2e257bf9b77ce9058e6fd6104782@65.21.16.240:24656,f26a69bb3c7e50af91796485d146dda83f44d147@185.189.45.16:26661,81f93b53fbc1b769fd562ad64178b18f636d2417@136.243.90.254:26656,fabfff6b159f1122f9101ee565f4ab0d8114498e@152.53.109.34:2080,0889ba91b29b68924cf9ccfaab6614d66ec020ba@64.130.49.141:22256,48af26f9e567365353865c8df78201da101eb9ed@34.254.180.188:30177,fc53acb3a25d9ad939d1e34b2a1fc8367a659c66@135.181.152.80:26656,2a3dc9e7abd028aac54a69b07ed746d45f9a443e@46.37.123.81:26656,b8ef5be39bf261414c014cf24e54b029d3a49481@65.108.198.145:25656,92e80e4f4d1103a7bcfe7258fb3a4cdf585def2d@65.21.15.179:12656,02a405e8182306bd0c4f39d0ebdd2aca0e787170@46.166.162.40:26656,de9d7f9e27740641880d3fb3eb88340a977da637@65.21.221.85:24656,3eb6b71d07273719d0093e288cdac6a4cc4935e3@65.108.233.3:25656,bd9f3d8323554c8e6a26862e94a3565745e2128e@185.119.118.113:3000,886e817dc293457f595525b6f4465813bac11072@198.244.182.197:26656,f51c9a0e2911c45eff81b2a4a6fee0fa38211dbd@152.53.66.205:3664,56f2b99440b7e7f540c07ca88bc8a87f6386b1d6@65.21.132.31:25656,5b5313396dea3a96dcd7e48af1d7666e1c0053c3@57.129.52.107:26656,7bc29fdaebf7f0a832820751a04e341af0e7ad27@66.45.237.130:26656,54c1f40e10e131a2775b2effa5aa300c099d4e00@188.165.77.153:26656,0e6469210b919827056c36b520c9d8819beae50b@160.250.106.199:22656,90b8ba2e54de12ac1244db72262ea0c5c27f5925@95.216.98.69:24656,6942fbc227ef76412e09ca434a0974b173d60f6a@216.105.169.42:14456,b7b9531ea5ad9727f3ffc595b37da78debec6515@65.21.234.111:17956,b34129cf09da8b01d6c4e2c1d4107c7baca65859@23.83.185.129:26656,a728501a933b3e4d017b32206ccf65f622ff626b@65.108.230.45:3000,e44340f514ff0ac549dd25655240be82dfc52c39@205.209.125.118:17156,04f27ef917d0ca4f8e007ea36fce2e31c98a8c29@184.107.244.74:18700,ea591466471636087ca0c142610e26fad4058490@37.27.58.244:38656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.union/config/config.toml
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.union/config/config.toml
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
s%:6065%:${APP_PORT}065%g" $HOME/.union/config/app.toml

# config.toml
sed -i.bak -e "s%:26658%:${APP_PORT}658%g;
s%:26657%:${APP_PORT}657%g;
s%:6060%:${APP_PORT}060%g;
s%:26656%:${APP_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${APP_PORT}656\"%;
s%:26660%:${APP_PORT}660%g" $HOME/.union/config/config.toml
```

---

## Pruning
```bash
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.union/config/app.toml 
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.union/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"10\"/" $HOME/.union/config/app.toml
```

---

## Min Gas, Prometheus, Indexer
```bash
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.025au"|g' $HOME/.union/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.union/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.union/config/config.toml
```

---

## Create Service File
```bash
sudo tee /etc/systemd/system/uniond.service > /dev/null <<EOF
[Unit]
Description=uniond Node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.union
ExecStart=$(which uniond) start --home $HOME/.union
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
sudo systemctl enable uniond
sudo systemctl restart uniond && sudo journalctl -u uniond -fo cat
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
uniond keys add $WALLET
```
`If u want to restore use this command`
```bash
uniond keys add $WALLET --recover
```
`Set keys variable enviroment`
```bash
WALLET_ADDRESS=$(uniond keys show $WALLET -a)
VALOPER_ADDRESS=$(uniond keys show $WALLET --bech val -a)
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
  \"pubkey\": {\"@type\": \"/cosmos.crypto.ed25519.PubKey\", \"key\": \"$(uniond tendermint show-validator | grep -Po '\"key\":\s*\"\K[^\"]*')\"},
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
uniond tx staking create-validator validator.json \
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
sudo systemctl stop uniond
sudo systemctl disable uniond
sudo systemctl daemon-reload
sudo rm -rf /etc/systemd/system/uniond.service
sudo rm $(which uniond)
sudo rm -rf $HOME/.union
```