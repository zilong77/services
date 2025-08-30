# 📑 Safrochain Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u safrochaind -fo cat
```

### Check service status
```bash
sudo systemctl status safrochaind
```

### Node info
```bash
safrochaind status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start safrochaind
```

### Stop service
```bash
sudo systemctl stop safrochaind
```

### Restart service
```bash
sudo systemctl restart safrochaind
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable safrochaind
```

### Disable service
```bash
sudo systemctl disable safrochaind
```

---

## 🔑 Key Management

### Add New Wallet
```bash
safrochaind keys add $WALLET
```

### Restore executing wallet
```bash
safrochaind keys add $WALLET --recover
```

### List All Wallets
```bash
safrochaind keys list
```

### Delete wallet
```bash
safrochaind keys delete $WALLET
```

### Check Balance
```bash
safrochaind q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
safrochaind keys export $WALLET
```

### View EVM Private Key
```bash
safrochaind keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
safrochaind keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
safrochaind tx distribution withdraw-all-rewards --from $WALLET --chain-id safro-testnet-1 --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

### Withdraw rewards and commission from your validator
```bash
safrochaind tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission  --chain-id safro-testnet-1   --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

### Check your balance
```bash
safrochaind query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
safrochaind tx staking delegate $(safrochaind keys show $WALLET --bech val -a) 1000000usaf --from $WALLET  --chain-id safro-testnet-1   --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

### Delegate
```bash
safrochaind tx staking delegate <TO_VALOPER_ADDRESS> 1000000usaf --from $WALLET --chain-id safro-testnet-1   --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

### Redelegate Stake to Another Validator
```bash
safrochaind tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000usaf --from $WALLET  --chain-id safro-testnet-1   --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

### Unbond
```bash
safrochaind tx staking unbond $(safrochaind keys show $WALLET --bech val -a) 1000000usaf --from $WALLET --chain-id safro-testnet-1 --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

### Transfer Funds
```bash
safrochaind tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000usaf --from $WALLET --chain-id safro-testnet-1   --gas auto --gas-adjustment 1.2 --fees 50000usaf -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
safrochaind status 2>&1 | jq
```

### Validator Details
```bash
safrochaind q staking validator $(safrochaind keys show $WALLET --bech val -a)
```

### Jailing info
```bash
safrochaind q slashing signing-info $(safrochaind tendermint show-validator)
```

### Slashing parameters
```bash
safrochaind q slashing params
```

### Unjail validator
```bash
safrochaind tx slashing unjail --from $WALLET --chain-id safro-testnet-1   --gas auto --gas-adjustment 1.2 --fees 50000usaf   -y
```

### Active Validators List
```bash
safrochaind q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(safrochaind q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(safrochaind status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
safrochaind q slashing signing-info $(safrochaind tendermint show-validator)
```

### Edit Validator
```bash
safrochaind tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $$WALLET \
--chain-id safro-testnet-1 \
--gas auto \
--gas-adjustment 1.2 \
--fees 50000usaf \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
safrochaind query gov proposals
```

### View proposal
```bash
safrochaind query gov proposal 1
```

### Vote
```bash
safrochaind tx gov vote 1 yes --from $$WALLET --chain-id safro-testnet-1 --gas auto --gas-adjustment 1.2 --fees 50000usaf   -y
```