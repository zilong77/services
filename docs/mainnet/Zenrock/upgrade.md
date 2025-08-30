# ⚡ Upgrade - Zenrock (Mainnet)

## Manual Upgrade

```bash
cd $HOME
wget -O zenrockd.zip https://github.com/Zenrock-Foundation/zrchain/releases/download/v6.25.0/zenrockd.zip
unzip zenrockd.zip
rm zenrockd.zip
chmod +x $HOME/zenrockd
sudo mv $HOME/zenrockd $(which zenrockd)
sudo systemctl restart zenrockd && sudo journalctl -u zenrockd -f
```
