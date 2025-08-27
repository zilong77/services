# ⚡ Upgrade - ChainA (Testnet)

Panduan melakukan upgrade untuk **ChainA Testnet**.

```bash
# stop service
sudo systemctl stop chainad

# download versi baru
wget https://example.com/chainad-v2 -O /usr/local/bin/chainad
chmod +x /usr/local/bin/chainad

# restart service
sudo systemctl start chainad
```
