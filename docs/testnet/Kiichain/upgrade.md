# ⚡ Upgrade - Kiichain (Testnet)

## Manual Upgrade

```bash
cd $HOME
sudo systemctl stop kiichaind
rm -rf kiichain
git clone https://github.com/KiiChain/kiichain.git
cd kiichain
git fetch --tags
git checkout v4.0.0
make build
mv $HOME/go/bin/kiichaind $HOME/go/bin/kiichaind_backup
cp build/kiichaind $HOME/go/bin/kiichaind
chmod +x $HOME/go/bin/kiichaind
kiichaind version
sudo systemctl start kiichaind
```
