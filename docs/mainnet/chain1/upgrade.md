# ⚡ Upgrade - Chain1

Panduan melakukan upgrade untuk **Chain1**.

```bash
# stop service
sudo systemctl stop chain1d

# download versi baru
wget https://example.com/chain1d-v2 -O /usr/local/bin/chain1d
chmod +x /usr/local/bin/chain1d

# restart service
sudo systemctl start chain1d
```
