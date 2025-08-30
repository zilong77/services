# 📑 Lumera Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u lumerad -fo cat
```

### Check service status
```bash
sudo systemctl status lumerad
```

### Node info
```bash
lumerad status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start lumerad
```

### Stop service
```bash
sudo systemctl stop lumerad
```

### Restart service
```bash
sudo systemctl restart lumerad
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable lumerad
```

### Disable service
```bash
sudo systemctl disable lumerad
```

---

## 🔑 Key Management

### Add New Wallet
```bash
lumerad keys add $WALLET
```

### Restore executing wallet
```bash
lumerad keys add $WALLET --recover
```

### List All Wallets
```bash
lumerad keys list
```

### Delete wallet
```bash
lumerad keys delete $WALLET
```

### Check Balance
```bash
lumerad q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
lumerad keys export $WALLET
```

### View EVM Private Key
```bash
lumerad keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
lumerad keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
lumerad tx distribution withdraw-all-rewards --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Withdraw rewards and commission from your validator
```bash
lumerad tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Check your balance
```bash
lumerad query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
lumerad tx staking delegate $(lumerad keys show $WALLET --bech val -a) 1000000ulume --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Delegate
```bash
lumerad tx staking delegate <TO_VALOPER_ADDRESS> 1000000ulume --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Redelegate Stake to Another Validator
```bash
lumerad tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000ulume --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Unbond
```bash
lumerad tx staking unbond $(lumerad keys show $WALLET --bech val -a) 1000000ulume --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Transfer Funds
```bash
lumerad tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000ulume --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
lumerad status 2>&1 | jq
```

### Validator Details
```bash
lumerad q staking validator $(lumerad keys show $WALLET --bech val -a)
```

### Jailing info
```bash
lumerad q slashing signing-info $(lumerad tendermint show-validator)
```

### Slashing parameters
```bash
lumerad q slashing params
```

### Unjail validator
```bash
lumerad tx slashing unjail --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```

### Active Validators List
```bash
lumerad q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(lumerad q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(lumerad status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
lumerad q slashing signing-info $(lumerad tendermint show-validator)
```

### Edit Validator
```bash
lumerad tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $WALLET \
--chain-id lumera-testnet-2 \
--fees 0.025ulume \
--gas-adjustment 1.3 \
--gas auto \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
lumerad query gov proposals
```

### View proposal
```bash
lumerad query gov proposal 1
```

### Vote
```bash
lumerad tx gov vote 1 yes --from $WALLET --chain-id lumera-testnet-2 --gas-prices=0.025ulume --gas-adjustment=1.5 --gas=auto -y
```