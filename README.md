# What is this?

This repository contains plugins and config examples for Munin. See [Munin/Wiki](http://munin-monitoring.org/wiki) for more detailed information.

# Configuration

## Agent configuration
* run `munin-node-configure` to show enabled and available plugins
* run `munin-node-configure --suggest` to show which plugins are suggested to be included in your config
* run `munin-node-configure --suggest --shell` to obtain shell comands to enable plugins.

To enable plugin in the agent create a sumbolic link to it:
```
ln -s '/usr/local/share/munin/plugins/df' '/etc/munin/plugins/df'
ln -s '/usr/local/share/munin/plugins/df_inode' '/etc/munin/plugins/df_inode'
ln -s '/usr/local/share/munin/plugins/diskstats' '/etc/munin/plugins/diskstats'
ln -s '/usr/local/share/munin/plugins/fw_packets' '/etc/munin/plugins/fw_packets'
ln -s '/usr/local/share/munin/plugins/if_' '/etc/munin/plugins/if_enp0s3'
```

Plugins are configured using env variables. Put plugin config files to files in /etc/munin/plugin-conf.d

## Testing plugins

You can run specific plugin with the config issuing:
`munin-run <plugin_name>`

Munin agent listens on tcp port 4949:
`telnet 0 4949`
After connecting, you can execute commands like `list` or `fetch <plugin_name>`

# Useful plugins

Useful plugins (a subjective selection):
* diskstats & iostat - IO stats for selected disks
* df_ - disk space and inode usage
* procfs - process stats
* memory - system memory
* apache - apacher traffic
* jmx_ - JMX statistics collector. 
* ip_ - monitor ip traffic. Allows monitoring traffic to a selected IP address or subnet. Based on ip tables
* tcp - tcp socket stats
* mysql - monitor mysql threads, queries and long queries
* postgres - monitor postgres database
* netstat - similiar to the tcp plugin, shows connections status. By default does not work in CentOS/RHEL 7
* nfs_client and nfsd - NFS server/client monitoring
* port_ - stats for selected TCP port
* rails - See [barttenbrinke/munin-plugins-rails](https://github.com/barttenbrinke/munin-plugins-rails)
 
# My plugins & examples

## IP Round trip time
Enable:
`ln -s '/usr/local/share/munin/plugins/ip_rtt_' '/etc/munin/plugins/ip_rtt_188.227.173.2'`

This is a wildcard plugin to monitor TCP round trip time for a specific host.

## TCP socket queue size for specific port
Enable:
`ln -s '/usr/local/share/munin/plugins/tcp_q_' '/etc/munin/plugins/tcp_q_80'`

This is a wildcard plugin to monitor TCP socket queue size for specified port. Queries send and receive TCP queue from both given port 
(both source and destination).


## DLM monitoring
DLM (distributed lock manager) is used by the RedHat Clustered file system (GFS2).

* Enable monitoring of tcp port 21064
`ln -s '/usr/local/share/munin/plugins/port_' '/usr/local/etc/munin/plugins/port_21064'`


