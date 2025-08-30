# ⚡ Upgrade - Safrochain (Testnet)

## Manual Upgrade

```bash
cd $HOME
sudo systemctl stop safrochaind
rm -rf safrochain-node
git clone https://github.com/Safrochain-Org/safrochain-node.git
cd safrochain-node
git checkout v0.1.0
make install
sudo systemctl start safrochaind
```
