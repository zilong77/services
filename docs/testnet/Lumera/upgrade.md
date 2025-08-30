# ⚡ Upgrade - Lumera (Testnet)

## Manual Upgrade

```bash
cd $HOME
sudo systemctl stop lumerad
wget https://github.com/LumeraProtocol/lumera/releases/download/v1.7.0/lumera_v1.7.0_linux_amd64.tar.gz
tar -xvf lumera_v1.7.0_linux_amd64.tar.gz
rm lumera_v1.7.0_linux_amd64.tar.gz
rm install.sh
chmod +x lumerad
mv lumerad $HOME/go/bin/
sudo systemctl start lumerad
```
