# 📑 Zenrock Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u kiichaind -fo cat
```

### Check service status
```bash
sudo systemctl status kiichaind
```

### Node info
```bash
kiichaind status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start kiichaind
```

### Stop service
```bash
sudo systemctl stop kiichaind
```

### Restart service
```bash
sudo systemctl restart kiichaind
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable kiichaind
```

### Disable service
```bash
sudo systemctl disable kiichaind
```

---

## 🔑 Key Management

### Add New Wallet
```bash
kiichaind keys add $WALLET
```

### Restore executing wallet
```bash
kiichaind keys add $WALLET --recover
```

### List All Wallets
```bash
kiichaind keys list
```

### Delete wallet
```bash
kiichaind keys delete $WALLET
```

### Check Balance
```bash
kiichaind q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
kiichaind keys export $WALLET
```

### View EVM Private Key
```bash
kiichaind keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
kiichaind keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
kiichaind tx distribution withdraw-all-rewards --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii
```

### Withdraw rewards and commission from your validator
```bash
kiichaind tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id oro_1336-1 --fees 1250000000akii -y
```

### Check your balance
```bash
kiichaind query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
kiichaind tx staking delegate $(kiichaind keys show $WALLET --bech val -a) 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y
```

### Delegate
```bash
kiichaind tx staking delegate <TO_VALOPER_ADDRESS> 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y
```

### Redelegate Stake to Another Validator
```bash
kiichaind tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y
```

### Unbond
```bash
kiichaind tx staking unbond $(kiichaind keys show $WALLET --bech val -a) 1000000000akii --from $WALLET --chain-id oro_1336-1 --fees 12500000000akii -y
```

### Transfer Funds
```bash
kiichaind tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000000akii --fees 1250000000akii -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
kiichaind status 2>&1 | jq
```

### Validator Details
```bash
kiichaind q staking validator $(kiichaind keys show $WALLET --bech val -a)
```

### Jailing info
```bash
kiichaind q slashing signing-info $(kiichaind tendermint show-validator)
```

### Slashing parameters
```bash
kiichaind q slashing params
```

### Unjail validator
```bash
kiichaind tx slashing unjail --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y
```

### Active Validators List
```bash
kiichaind q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(kiichaind q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(kiichaind status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
kiichaind q slashing signing-info $(kiichaind tendermint show-validator)
```

### Edit Validator
```bash
kiichaind tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $WALLET \
--chain-id oro_1336-1 \
--fees 1250000000akii \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
kiichaind query gov proposals
```

### View proposal
```bash
kiichaind query gov proposal 1
```

### Vote
```bash
kiichaind tx gov vote 1 yes --from $WALLET --chain-id oro_1336-1 --fees 1250000000akii -y
```