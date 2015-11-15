# What is this?

This repository contains plugins and config examples for Munin. See [Munin/Wiki](http://munin-monitoring.org/wiki) for more detailed information.

# Configuration

## Plugin config
* run `munin-node-configure` to show enabled and available plugins
* run `munin-node-configure --suggest` to show which plugins are suggested to be included in your config
* run `munin-node-configure --suggest --shell` to obtain shell comands to enable plugins.

To enable plugin create a sumbolic link to it:
```
ln -s '/usr/local/share/munin/plugins/df' '/etc/munin/plugins/df'
ln -s '/usr/local/share/munin/plugins/df_inode' '/etc/munin/plugins/df_inode'
ln -s '/usr/local/share/munin/plugins/diskstats' '/etc/munin/plugins/diskstats'
ln -s '/usr/local/share/munin/plugins/fw_packets' '/etc/munin/plugins/fw_packets'
ln -s '/usr/local/share/munin/plugins/if_' '/etc/munin/plugins/if_enp0s3'
```

Plugins are configured using env variables. Put plugin config files to files in /etc/munin/plugin-conf.d

## Test plugin

You can run specific plugin with the config issuing:
`munin-run <plugin_name>`

Munin agent listens on tcp port 4949:
`telnet 0 4949`

# Plugins

Usable and interesting plugins (a subjective selection):
* diskstats - IO stats for selected disks
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