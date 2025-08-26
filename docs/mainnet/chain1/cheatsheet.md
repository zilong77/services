# 📑 Cheat Sheet - Chain1

Beberapa command penting untuk validator Chain1.

```bash
# cek status service
sudo systemctl status chain1d

# cek logs
journalctl -fu chain1d -o cat

# sync info
chain1d status 2>&1 | jq .SyncInfo

# cek validator info
chain1d q staking validator <valoper_address>
```
