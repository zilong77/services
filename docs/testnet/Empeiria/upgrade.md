# ⚡ Upgrade - Empeiria (Testnet)

## Manual Upgrade

```bash
cd $HOME
rm -rf bin
mkdir bin
cd $HOME/bin
curl -LO https://github.com/empe-io/empe-chain-releases/raw/master/v0.4.0/emped_v0.4.0_linux_amd64.tar.gz
tar -xvf emped_v0.4.0_linux_amd64.tar.gz
chmod +x $HOME/bin/emped
sudo mv $HOME/bin/emped $(which emped)
sudo systemctl restart emped && sudo journalctl -u emped -f
```
