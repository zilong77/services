# ⚡ Upgrade - Paxi (Mainnet)

## Manual Upgrade

```bash
sudo systemctl stop paxid
cd $HOME
rm -rf paxi
git clone https://github.com/paxi-web3/paxi.git
cd paxi
git checkout v1.0.6
go build -mod=readonly -tags "cosmwasm pebbledb" -o $HOME/go/bin/paxid ./cmd/paxid
paxid version
sudo systemctl start paxid && sudo journalctl -u paxid -fo cat
```
