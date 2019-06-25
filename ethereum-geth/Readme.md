Ubuntu 18.04 is recommended

Create user:
```
sudo useradd --create-home eth
sudo loginctl enable-linger eth
sudo -i -u eth
```

```
mkdir -p .config/systemd/user/
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/ethereum-geth/geth-mainnet.service -O ~/.config/systemd/user/geth-mainnet.service

mkdir -p ~/bin
cd ~/bin
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.8.27-4bcc0a37.tar.gz
tar -xf geth-linux-amd64-1.8.27-4bcc0a37.tar.gz
```

Start it up:
```
export XDG_RUNTIME_DIR="/run/user/$UID"
export DBUS_SESSION_BUS_ADDRESS="unix:path=${XDG_RUNTIME_DIR}/bus"

systemctl --user daemon-reload
systemctl --user enable geth-mainnet.service
systemctl --user start geth-mainnet.service
```
Check if node is running well: 
`ps auxww | fgrep geth` should return process with full agruments and metadata.
If not user `journalctl --user -u geth-mainnet.service` to check logs

