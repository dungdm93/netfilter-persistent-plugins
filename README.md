`ipvs-persistent`
=================
`ipvs-persistent` is a `netfilter-persistent` plugin to auto-load IPVS rules on boot.

## 1. Installation
* Install `ipvsadm` and `netfilter-persistent` packages
```bash
# only on Debian-based OS like Ubuntu, Linux Mint
apt install ipvsadm netfilter-persistent

# enable netfilter-persistent start on boot
systemctl enable netfilter-persistent
```

* Dowload `ipvs-persistent` plugin
```bash
mkdir -p /etc/ipvs

curl -L https://github.com/dungdm93/ipvs-persistent/raw/master/32-ipvsadm \
     -o /usr/share/netfilter-persistent/plugins.d/32-ipvsadm
chmod +x /usr/share/netfilter-persistent/plugins.d/32-ipvsadm
```

## 2. Usage
* Save current IPVS rules
```bash
ipvsadm --save --numeric > /etc/ipvs/rules
```

* **Your IPVS rules are now persisted, and automatically load on boot.**  
If you'd like to load/reset IPVS rules:
```bash
service netfilter-persistent reload

# --or--
systemctl restart netfilter-persistent
```

* *BONUS*: some others helpful commands
```bash
# List  the virtual server table
ipvsadm -l -n

# Clear the virtual server table.
ipvsadm --clear
```
