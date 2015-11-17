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
`ln -s '/usr/local/share/munin/plugins/port_' '/etc/munin/plugins/port_21064'`

## GFS monitoring

Use wildcard gfs2_ plugin from this repo:
This plugin does not normaly require configuration. This is a wildcard plugin to fetch GFS2 debugfs counters.

This plugin should be linked as:
gfs2_${mode}_${data}_${filesystem}

Where:
mode - glocks or holders 
data - state type flag. Type only for glocks

Example:
`ln -s '/usr/local/share/munin/plugins/gfs2_' '/etc/munin/plugins/gfs2_glocks_state_cluster:lunLogVol01'`

Documentation:
http://lxr.free-electrons.com/source/Documentation/filesystems/gfs2-glocks.txt
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Global_File_System_2/ap-glock-debugfs-gfs2.html


## Dell Storage Monitoring
The MD3000i does not expose any performance data via SNMP. You can use "SMCli" which comes on the resource CD to get some basic stats from the MD3000i. 

`/opt/dell/mdstoragemanager/client/SMcli <IP> -S -quick -c "show virtualDisk [diskname] performanceStats;"` 

The output looks like this: 
```
"Performance Monitor Statistics for Storage Array: md3000i - Date/Time: 8/31/09 1:38:39 PM - Polling interval in seconds: 5" 

"Storage Arrays ","Total IOs ","Read Percentage ","Cache Hit Percentage ","Current KB/second ","Maximum KB/second ","Current IO/second ","Maximum IO/second" 

"Capture Iteration: 1","","","","","","","" 
"Date/Time: 8/31/09 1:38:39 PM","","","","","","","" 
"CONTROLLER IN SLOT 0","106.0","3.8","25.0","161.6","161.6","21.2","21.2" 
"Virtual Disk diskname","106.0","3.8","25.0","161.6","161.6","21.2","21.2" 
"CONTROLLER IN SLOT 1","0.0","0.0","0.0","0.0","0.0","0.0","0.0" 
"STORAGE ARRAY TOTALS","106.0","3.8","25.0","161.6","161.6","21.2","21.2" 
```

The status returned is for the last 5 seconds only. No counters are available.

Enable:
`ln -s '/usr/local/share/munin/plugins/md3200' '/etc/munin/plugins/md3200'`
Add configuration to /etc/munin/plugins/md3200:
```
[md3200]
  env.controller 192.168.0.1
  env.smcli /opt/dell/mdstoragemanager/client/SMcli
  env.disk disk1
```

