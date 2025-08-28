# 📑 Kiichain Cheat Sheet

## 🔧 Service Operations
**USAGE** | **COMMAND**
---|---
Check logs | `sudo journalctl -u kiichaind -fo cat`<br>
Check service status | `sudo systemctl status kiichaind`<br>
Node info | `kiichaind status 2>&1 | jq`<br>
Start service | `sudo systemctl start kiichaind`<br>
Stop service | `sudo systemctl stop kiichaind`<br>
Restart service | `sudo systemctl restart kiichaind`<br>
Reload services | `sudo systemctl daemon-reload`<br>
Enable Service | `sudo systemctl enable kiichaind`<br>
Disable Service | `sudo systemctl disable kiichaind`<br>

---

## 🔑 Key Management
**USAGE** | **COMMAND**
---|---
Add New Wallet | `kiichaind keys add $WALLET`<br>
Restore executing wallet | `kiichaind keys add $WALLET --recover`<br>
List All Wallets | `kiichaind keys list`<br>
Delete wallet | `kiichaind keys delete $WALLET`<br>
Check Balance | `kiichaind q bank balances $WALLET_ADDRESS`<br>
Export Key (save to wallet.backup) | `kiichaind keys export $WALLET`<br>
View EVM Private Key | `kiichaind keys unsafe-export-eth-key $WALLET`<br>
Import Key (restore from wallet.backup) | `kiichaind keys import $WALLET wallet.backup`<br>

---

## 💰 Tokens
**USAGE** | **COMMAND**
---|---
Withdraw all rewards | `kiichaind tx distribution withdraw-all-rewards --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii`<br>
Withdraw rewards and commission from your validator | `kiichaind tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id oro_1336-1 --fees 1250000000akii -y`<br>
Check your balance | `kiichaind query bank balances $WALLET_ADDRESS`<br>
Delegate to Yourself | `kiichaind tx staking delegate $(kiichaind keys show $WALLET --bech val -a) 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`<br>
Delegate | `kiichaind tx staking delegate <TO_VALOPER_ADDRESS> 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`<br>
Redelegate Stake to Another Validator | `kiichaind tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`<br>
Unbond | `kiichaind tx staking unbond $(kiichaind keys show $WALLET --bech val -a) 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 12500000000akii -y`<br>
Transfer Funds | `kiichaind tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000000akii --fees 1250000000akii -y`<br>

---

## 🛡️ Validator Operations
**USAGE** | **COMMAND**
---|---
Validator info | `kiichaind status 2>&1 | jq`<br>
Validator Details | `kiichaind q staking validator $(kiichaind keys show $WALLET --bech val -a)`<br>
Jailing info | `kiichaind q slashing signing-info $(kiichaind tendermint show-validator)`<br>
Slashing parameters | `kiichaind q slashing params`<br>
Unjail validator | `kiichaind tx slashing unjail --from $WALLET --chain-id oro_1336-1 --fees xxxxxxxx -y`<br>
Active Validators List | `kiichaind q staking validators -oj --limit=2000 \ | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' \ | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' \ | sort -gr | nl`<br>
Check Validator key | `[[ $(kiichaind q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(kiichaind status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"`<br>
Signing info | `kiichaind q slashing signing-info $(kiichaind tendermint show-validator)`<br>

---

## 🏛 Governance
**USAGE** | **COMMAND**
---|---
Proposals List | `kiichaind query gov proposals`<br>
View proposal | `kiichaind query gov proposal 1`<br>
Vote | `kiichaind tx gov vote 1 yes --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`<br>
