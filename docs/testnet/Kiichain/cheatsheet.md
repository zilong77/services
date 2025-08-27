# 📑 Cosmos Node Cheat Sheet (Template)

## 🔧 Service Operations
**USAGE** | **COMMAND**
---|---
Check logs | `sudo journalctl -u kiichaind -fo cat`
Check service status | `sudo systemctl status kiichaind`
Node info | `kiichaind status 2>&1 | jq`
Start service | `sudo systemctl start kiichaind`
Stop service | `sudo systemctl stop kiichaind`
Restart service | `sudo systemctl restart kiichaind`
Reload services | `sudo systemctl daemon-reload`
Enable Service | `sudo systemctl enable kiichaind`
Disable Service | `sudo systemctl disable kiichaind`

---

## 🔑 Key Management
**USAGE** | **COMMAND**
---|---
Add New Wallet | `kiichaind keys add $WALLET`
Restore executing wallet | `kiichaind keys add $WALLET --recover`
List All Wallets | `kiichaind keys list`
Delete wallet | `kiichaind keys delete $WALLET`
Check Balance | `kiichaind q bank balances $WALLET_ADDRESS`
Export Key (save to wallet.backup) | `kiichaind keys export $WALLET`
View EVM Private Key | `kiichaind keys unsafe-export-eth-key $WALLET`
Import Key (restore from wallet.backup) | `kiichaind keys import $WALLET wallet.backup`

---

## 💰 Tokens
**USAGE** | **COMMAND**
---|---
Withdraw all rewards | `kiichaind tx distribution withdraw-all-rewards --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii`
Withdraw rewards and commission from your validator | `kiichaind tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id oro_1336-1 --fees 1250000000akii -y`
Check your balance | `kiichaind query bank balances $WALLET_ADDRESS`
Delegate to Yourself | `kiichaind tx staking delegate $(kiichaind keys show $WALLET --bech val -a) 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`
Delegate | `kiichaind tx staking delegate <TO_VALOPER_ADDRESS> 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`
Redelegate Stake to Another Validator | `kiichaind tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y`
Unbond | `kiichaind tx staking unbond $(kiichaind keys show $WALLET --bech val -a) 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 12500000000akii -y`
Transfer Funds | `kiichaind tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000000akii --fees 1250000000akii -y`

---

## 🛡️ Validator Operations
**USAGE** | **COMMAND**
---|---
Validator info | `kiichaind status 2>&1 | jq`
Validator Details | `kiichaind q staking validator $(kiichaind keys show $WALLET --bech val -a)`
Jailing info | `kiichaind q slashing signing-info $(kiichaind tendermint show-validator)`
Slashing parameters | `kiichaind q slashing params`
Unjail validator | `kiichaind tx slashing unjail --from $WALLET --chain-id oro_1336-1 --fees xxxxxxxx -y`
Active Validators List | `kiichaind q staking validators -oj --limit=2000 \
  | jq '.validators[] | select(.status==\"BOND_STATUS_BONDED\")' \
  | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' \
  | sort -gr | nl`
Check Validator key | `[[ $(kiichaind q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) \
= $(kiichaind status | jq -r .ValidatorInfo.PubKey.value) ]] \
&& echo -e "Your key status is ok" \
|| echo -e "Your key status is error"`
Signing info | `kiichaind q slashing signing-info $(kiichaind tendermint show-validator)`

---

## 🏛 Governance
**USAGE** | **COMMAND**
---|---
Proposals List | `kiichaind query gov proposals`
View proposal | `kiichaind query gov proposal 1`
Vote | `kiichaind tx gov vote 1 yes --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y``
