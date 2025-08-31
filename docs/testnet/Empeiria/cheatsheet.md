# 📑 Empeiria Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u emped -fo cat
```

### Check service status
```bash
sudo systemctl status emped
```

### Node info
```bash
emped status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start emped
```

### Stop service
```bash
sudo systemctl stop emped
```

### Restart service
```bash
sudo systemctl restart emped
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable emped
```

### Disable service
```bash
sudo systemctl disable emped
```

---

## 🔑 Key Management

### Add New Wallet
```bash
emped keys add $WALLET
```

### Restore executing wallet
```bash
emped keys add $WALLET --recover
```

### List All Wallets
```bash
emped keys list
```

### Delete wallet
```bash
emped keys delete $WALLET
```

### Check Balance
```bash
emped q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
emped keys export $WALLET
```

### View EVM Private Key
```bash
emped keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
emped keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
emped tx distribution withdraw-all-rewards --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Withdraw rewards and commission from your validator
```bash
emped tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Check your balance
```bash
emped query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
emped tx staking delegate $(emped keys show $WALLET --bech val -a) 1000000uempe --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Delegate
```bash
emped tx staking delegate <TO_VALOPER_ADDRESS> 1000000uempe --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Redelegate Stake to Another Validator
```bash
emped tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000uempe --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Unbond
```bash
emped tx staking unbond $(emped keys show $WALLET --bech val -a) 1000000uempe --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Transfer Funds
```bash
emped tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000uempe --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
emped status 2>&1 | jq
```

### Validator Details
```bash
emped q staking validator $(emped keys show $WALLET --bech val -a)
```

### Jailing info
```bash
emped q slashing signing-info $(emped tendermint show-validator)
```

### Slashing parameters
```bash
emped q slashing params
```

### Unjail validator
```bash
emped tx slashing unjail --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```

### Active Validators List
```bash
emped q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(emped q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(emped status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
emped q slashing signing-info $(emped tendermint show-validator)
```

### Edit Validator
```bash
emped tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $WALLET \
--chain-id empe-testnet-2 \
--gas auto \
--gas-adjustment 1.5 \
--fees 30uempe \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
emped query gov proposals
```

### View proposal
```bash
emped query gov proposal 1
```

### Vote
```bash
emped tx gov vote 1 yes --from $WALLET --chain-id empe-testnet-2 --gas auto --gas-adjustment 1.5 --fees 30uempe -y
```