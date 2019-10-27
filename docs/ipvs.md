`ipvs-persistent`
=================

## 1. Installation
* Install `ipvsadm` and `netfilter-persistent` packages
    ```bash
    apt install ipvsadm netfilter-persistent

    # make sure netfilter-persistent is enabled
    systemctl is-enabled netfilter-persistent
    ```

* Setup `ipvs` plugin
    ```bash
    mkdir -p /etc/ipvs/

    curl -L https://github.com/dungdm93/ipvs-persistent/raw/master/plugins.d/32-ipvsadm \
        -o /usr/share/netfilter-persistent/plugins.d/32-ipvsadm
    chmod +x /usr/share/netfilter-persistent/plugins.d/32-ipvsadm
    ```

## 2. Usage
* Save current IPVS rules
    ```bash
    ipvsadm --save --numeric > /etc/ipvs/rules
    ```

    `/etc/ipvs/rules` will look like:
    ```
    -A -t 123.45.67.89:80 -s rr
    -a -t 123.45.67.89:80 -r 192.168.10.1:80 -m
    -a -t 123.45.67.89:80 -r 192.168.10.2:80 -m
    -a -t 123.45.67.89:80 -r 192.168.10.3:80 -m
    -a -t 123.45.67.89:80 -r 192.168.10.4:80 -m
    -a -t 123.45.67.89:80 -r 192.168.10.5:80 -m
    ```
    as you can see, each line actually a `ipvsadm` command without the entrypoint.

* **Your IPVS rules are now persisted, and automatically load on boot.**  
If you'd like to manual load/reset IPVS rules, run this command:
    ```bash
    service netfilter-persistent reload

    # --or--
    systemctl restart netfilter-persistent

    # --or--
    ipvsadm --restore < /etc/ipvs/rules
    ```

* *BONUS*: some others helpful commands
    ```bash
    # List  the virtual server table
    ipvsadm -l -n

    # Clear the virtual server table.
    ipvsadm --clear
    ```
