Ubuntu 18.04 is recommended

Create user and install packages:

```
useradd --create-home btc
mkdir ~btc/.ssh
loginctl enable-linger btc
apt install python3-pip
```
Login as btc user (sudo/su/ssh) 
Download distributives:
```
wget https://bitcoin.org/bin/bitcoin-core-0.18.0/bitcoin-0.18.0-x86_64-linux-gnu.tar.gz
tar -xf bitcoin-0.18.0-x86_64-linux-gnu.tar.gz
ln -s bitcoin-0.18.0 bitcoin

wget https://github.com/kyuupichan/electrumx/archive/1.12.0.tar.gz
tar -xf 1.12.0.tar.gz
ln -s electrumx-1.12.0 electrumx
pip3 install websockets plyvel aiorpcX aiohttp async-timeout pylru attrs
```

Now need to setup some configs
```
mkdir -p ~/.bitcoin
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/bitcoin/bitcoin.conf -O ~/.bitcoin/bitcoin.conf
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/bitcoin/.electrumx.conf -O ~/.electrumx.conf

mkdir -p ~/.config/systemd/user/
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/bitcoin/bitcoin.service -O ~/.config/systemd/user/bitcoin.service
wget https://raw.githubusercontent.com/vlddm/cryptonodes-setup/master/bitcoin/electrumx.service -O ~/.config/systemd/user/bitcoin/electrumx.service
```

Start it up:
```
export XDG_RUNTIME_DIR="/run/user/$UID"
export DBUS_SESSION_BUS_ADDRESS="unix:path=${XDG_RUNTIME_DIR}/bus"
systemctl --user daemon-reload
systemctl --user start bitcoin.service
systemctl --user start electrumx.service
```

See if its ok:
```
ps auxww | fgrep bitcoind
ps auxww | fgrep electrumx_server
tail -f ~/.bitcoin/debug.log
tail -f ~/.electrumxdb/log.txt 
```
