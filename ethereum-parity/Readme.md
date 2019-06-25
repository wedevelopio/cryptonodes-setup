Ubuntu 18.04 is recommended

Create user:
```
sudo useradd --create-home eth
sudo loginctl enable-linger eth
sudo -i -u eth
```

Fullnode setup:
```
mkdir -p ~/.local/share/io.parity.ethereum/
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/ethereum-parity/config.toml -O ~/.local/share/io.parity.ethereum/config.toml
```

Next:
```
mkdir -p .config/systemd/user/
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/ethereum-parity/parity.service -O .config/systemd/user/parity.service

mkdir -p ~/bin
cd ~/bin
wget https://releases.parity.io/ethereum/v2.4.7/x86_64-unknown-linux-gnu/parity
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.8.27-4bcc0a37.tar.gz
tar -xf geth-linux-amd64-1.8.27-4bcc0a37.tar.gz
```

Start it up:
```
export XDG_RUNTIME_DIR="/run/user/$UID"
export DBUS_SESSION_BUS_ADDRESS="unix:path=${XDG_RUNTIME_DIR}/bus"

systemctl --user daemon-reload
systemctl --user enable parity.service
systemctl --user start parity.service
```
Check if node is running well: 
`ps auxww | fgrep parity` should return process with full agruments and metadata.
If not user `journalctl --user -u parity.service` to check logs

