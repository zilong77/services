# 📑 Cheat Sheet - ChainA (Testnet)

Beberapa command penting untuk validator ChainA Testnet.

```bash
# cek logs service
journalctl -fu chainad -o cat

# cek sync
chainad status 2>&1 | jq .SyncInfo

# cek peers
curl -s localhost:26657/net_info | jq .result.peers[]
```
