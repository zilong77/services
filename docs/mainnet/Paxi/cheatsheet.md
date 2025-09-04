# 📑 Paxi Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u paxid -fo cat
```

### Check service status
```bash
sudo systemctl status paxid
```

### Node info
```bash
paxid status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start paxid
```

### Stop service
```bash
sudo systemctl stop paxid
```

### Restart service
```bash
sudo systemctl restart paxid
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable paxid
```

### Disable service
```bash
sudo systemctl disable paxid
```

---

## 🔑 Key Management

### Add New Wallet
```bash
paxid keys add $WALLET
```

### Restore executing wallet
```bash
paxid keys add $WALLET --recover
```

### List All Wallets
```bash
paxid keys list
```

### Delete wallet
```bash
paxid keys delete $WALLET
```

### Check Balance
```bash
paxid q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
paxid keys export $WALLET
```

### View EVM Private Key
```bash
paxid keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
paxid keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
paxid tx distribution withdraw-all-rewards --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Withdraw rewards and commission from your validator
```bash
paxid tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Check your balance
```bash
paxid query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
paxid tx staking delegate $(paxid keys show $WALLET --bech val -a) 1000000upaxi --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Delegate
```bash
paxid tx staking delegate <TO_VALOPER_ADDRESS> 1000000upaxi --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Redelegate Stake to Another Validator
```bash
paxid tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000upaxi --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Unbond
```bash
paxid tx staking unbond $(paxid keys show $WALLET --bech val -a) 1000000upaxi --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Transfer Funds
```bash
paxid tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000upaxi --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
paxid status 2>&1 | jq
```

### Validator Details
```bash
paxid q staking validator $(paxid keys show $WALLET --bech val -a)
```

### Jailing info
```bash
paxid q slashing signing-info $(paxid tendermint show-validator)
```

### Slashing parameters
```bash
paxid q slashing params
```

### Unjail validator
```bash
paxid tx slashing unjail --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```

### Active Validators List
```bash
paxid q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(paxid q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(paxid status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
paxid q slashing signing-info $(paxid tendermint show-validator)
```

### Edit Validator
```bash
paxid tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $WALLET \
--chain-id paxi-mainnet \
--fees 15000upaxi \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
paxid query gov proposals
```

### View proposal
```bash
paxid query gov proposal 1
```

### Vote
```bash
paxid  tx gov vote 1 yes --from $WALLET --chain-id paxi-mainnet --gas auto --gas-adjustment 1.5 --fees 15000upaxi -y
```