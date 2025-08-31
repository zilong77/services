# 📑 Zenrock Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u zenrockd -fo cat
```

### Check service status
```bash
sudo systemctl status zenrockd
```

### Node info
```bash
zenrockd status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start zenrockd
```

### Stop service
```bash
sudo systemctl stop zenrockd
```

### Restart service
```bash
sudo systemctl restart zenrockd
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable zenrockd
```

### Disable service
```bash
sudo systemctl disable zenrockd
```

---

## 🔑 Key Management

### Add New Wallet
```bash
zenrockd keys add $WALLET
```

### Restore executing wallet
```bash
zenrockd keys add $WALLET --recover
```

### List All Wallets
```bash
zenrockd keys list
```

### Delete wallet
```bash
zenrockd keys delete $WALLET
```

### Check Balance
```bash
zenrockd q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
zenrockd keys export $WALLET
```

### View EVM Private Key
```bash
zenrockd keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
zenrockd keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
zenrockd tx distribution withdraw-all-rewards --from $WALLET --chain-id gardia-9 --fees --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Withdraw rewards and commission from your validator
```bash
zenrockd tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Check your balance
```bash
zenrockd query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
zenrockd tx staking delegate $(zenrockd keys show $WALLET --bech val -a) 1000000urock --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Delegate
```bash
zenrockd tx staking delegate <TO_VALOPER_ADDRESS> 1000000urock --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Redelegate Stake to Another Validator
```bash
zenrockd tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000urock --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Unbond
```bash
zenrockd tx staking unbond $(zenrockd keys show $WALLET --bech val -a) 1000000urock --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Transfer Funds
```bash
zenrockd tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000urock --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
zenrockd status 2>&1 | jq
```

### Validator Details
```bash
zenrockd q staking validator $(zenrockd keys show $WALLET --bech val -a)
```

### Jailing info
```bash
zenrockd q slashing signing-info $(zenrockd tendermint show-validator)
```

### Slashing parameters
```bash
zenrockd q slashing params
```

### Unjail validator
```bash
zenrockd tx slashing unjail --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```

### Active Validators List
```bash
zenrockd q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(zenrockd q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(zenrockd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
zenrockd q slashing signing-info $(zenrockd tendermint show-validator)
```

### Edit Validator
```bash
zenrockd tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $WALLET \
--chain-id gardia-9 \
--gas auto \
--gas-adjustment 1.5 \
--fees 30urock \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
zenrockd query gov proposals
```

### View proposal
```bash
zenrockd query gov proposal 1
```

### Vote
```bash
zenrockd tx gov vote 1 yes --from $WALLET --chain-id gardia-9 --gas auto --gas-adjustment 1.5 --fees 30urock -y
```