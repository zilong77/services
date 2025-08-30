# 📑 Bitbadges Cheat Sheet

## 🔧 Service Operations

### Check logs
```bash
sudo journalctl -u bitbadgeschaind -fo cat
```

### Check service status
```bash
sudo systemctl status bitbadgeschaind
```

### Node info
```bash
bitbadgeschaind status 2>&1 | jq
```

### Start service
```bash
sudo systemctl start bitbadgeschaind
```

### Stop service
```bash
sudo systemctl stop bitbadgeschaind
```

### Restart service
```bash
sudo systemctl restart bitbadgeschaind
```

### Reload services
```bash
sudo systemctl daemon-reload
```

### Enable service
```bash
sudo systemctl enable bitbadgeschaind
```

### Disable service
```bash
sudo systemctl disable bitbadgeschaind
```

---

## 🔑 Key Management

### Add New Wallet
```bash
bitbadgeschaind keys add $WALLET
```

### Restore executing wallet
```bash
bitbadgeschaind keys add $WALLET --recover
```

### List All Wallets
```bash
bitbadgeschaind keys list
```

### Delete wallet
```bash
bitbadgeschaind keys delete $WALLET
```

### Check Balance
```bash
bitbadgeschaind q bank balances $WALLET_ADDRESS
```

### Export Key (save to wallet.backup)
```bash
bitbadgeschaind keys export $WALLET
```

### View EVM Private Key
```bash
bitbadgeschaind keys unsafe-export-eth-key $WALLET
```

### Import Key (restore from wallet.backup)
```bash
bitbadgeschaind keys import $WALLET wallet.backup
```

---

## 💰 Tokens

### Withdraw all rewards
```bash
bitbadgeschaind tx distribution withdraw-all-rewards --from $WALLET --chain-id bitbadges-1 --fees 0.025ubadge
```

### Withdraw rewards and commission from your validator
```bash
bitbadgeschaind tx distribution withdraw-rewards $VALOPER_ADDRESS --from $WALLET --commission --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

### Check your balance
```bash
bitbadgeschaind query bank balances $WALLET_ADDRESS
```

### Delegate to Yourself
```bash
bitbadgeschaind tx staking delegate $(bitbadgeschaind keys show $WALLET --bech val -a) 1000000000ubadge --from $WALLET --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

### Delegate
```bash
bitbadgeschaind tx staking delegate <TO_VALOPER_ADDRESS> 1000000000ubadge --from $WALLET --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

### Redelegate Stake to Another Validator
```bash
bitbadgeschaind tx staking redelegate $VALOPER_ADDRESS <TO_VALOPER_ADDRESS> 1000000000ubadge --from $WALLET --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

### Unbond
```bash
bitbadgeschaind tx staking unbond $(bitbadgeschaind keys show $WALLET --bech val -a) 1000000000ubadge --from $WALLET --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

### Transfer Funds
```bash
bitbadgeschaind tx bank send $WALLET_ADDRESS <TO_WALLET_ADDRESS> 1000000000ubadge --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

---

## 🛡️ Validator Operations

### Validator info
```bash
bitbadgeschaind status 2>&1 | jq
```

### Validator Details
```bash
bitbadgeschaind q staking validator $(bitbadgeschaind keys show $WALLET --bech val -a)
```

### Jailing info
```bash
bitbadgeschaind q slashing signing-info $(bitbadgeschaind tendermint show-validator)
```

### Slashing parameters
```bash
bitbadgeschaind q slashing params
```

### Unjail validator
```bash
bitbadgeschaind tx slashing unjail --from $WALLET --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```

### Active Validators List
```bash
bitbadgeschaind q staking validators -oj --limit=2000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10;6)|floor|tostring) + " " + .description.moniker' | sort -gr | nl
```

### Check Validator key
```bash
[[ $(bitbadgeschaind q staking validator $VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(bitbadgeschaind status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "Your key status is ok" || echo -e "Your key status is error"
```

### Signing info
```bash
bitbadgeschaind q slashing signing-info $(bitbadgeschaind tendermint show-validator)
```

### Edit Validator
```bash
bitbadgeschaind tx staking edit-validator \
--commission-rate 0.05 \
--new-moniker "$MONIKER" \
--identity "" \
--details "Kyronode for all" \
--from $WALLET \
--chain-id bitbadges-1 \
--fees 0.025ubadge \
-y
```

---

## 🏛 Governance

### Proposals List
```bash
bitbadgeschaind query gov proposals
```

### View proposal
```bash
bitbadgeschaind query gov proposal 1
```

### Vote
```bash
bitbadgeschaind tx gov vote 1 yes --from $WALLET --chain-id bitbadges-1 --gas auto --gas-adjustment 1.3 --fees 0.025ubadge -y
```