`ipset-persistent`
==================

## 1. Installation
* Install `ipset` and `netfilter-persistent` packages
    ```bash
    apt install ipset netfilter-persistent

    # make sure netfilter-persistent is enabled
    systemctl is-enabled netfilter-persistent
    ```

* Setup `ipset` plugin
    ```bash
    mkdir -p /etc/ipset/

    curl -L https://github.com/dungdm93/ipvs-persistent/raw/master/plugins.d/10-ipset \
        -o /usr/share/netfilter-persistent/plugins.d/10-ipset
    chmod +x /usr/share/netfilter-persistent/plugins.d/10-ipset
    ```

## 2. Usage
* Save current IPVS rules
    ```bash
    ipset save -file /etc/ipset/rules
    ```

    `/etc/ipset/rules` will look like:
    ```
    create limited_hosts hash:ip family inet hashsize 1024 maxelem 65536
    add limited_hosts 10.0.0.5
    add limited_hosts 10.0.0.6
    add limited_hosts 10.0.0.7

    create allowed_sites hash:ip family inet hashsize 1024 maxelem 65536
    add allowed_sites 125.212.247.216
    add allowed_sites 172.217.161.142
    add allowed_sites 123.30.151.74
    ```
    as you can see, each line actually a `ipset` command without the entrypoint.

* **Your IPset rules are now persisted, and automatically load on boot.**  
If you'd like to manual load/reset IPset rules, run this command:
    ```bash
    service netfilter-persistent reload

    # --or--
    systemctl restart netfilter-persistent

    # --or--
    ipset restore -file /etc/ipset/rules
    ```

* *BONUS*: some others helpful commands
    ```bash
    # List the header data and the entries for the specified set, or for all sets if none is given.
    ipset list [SETNAME]

    # Clear all entries from the specified set or flush all sets if none is given.
    ipset flush [SETNAME]

    # Destroy the specified set or all the sets if none is given.
    ipset destroy [SETNAME]
    ```
