# ⚡ Upgrade - Bitbadges (Mainnet)

## Manual Upgrade

```bash
cd $HOME
sudo systemctl stop bitbadgeschaind
wget https://github.com/BitBadges/bitbadgeschain/releases/download/v13/bitbadgeschain-linux-amd64
chmod +x $HOME/bitbadgeschain-linux-amd64
sudo mv bitbadgeschain-linux-amd64 $(bitbadgeschaind)
bitbadgeschaind version
sudo systemctl start bitbadgeschaind && sudo journalctl -u bitbadgeschaind -fo cat
```
