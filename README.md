# Pacemaker module for puppet

This module manages pacemaker on Linux distros with the ccs tool.

## Description

This module uses the fact osfamily which is supported by Facter 1.6.1+. If you do not have facter 1.6.1 in your environment, the following manifests will provide the same functionality in site.pp (before declaring any node):

    if ! $::osfamily {
      case $::operatingsystem {
        'RedHat', 'Fedora', 'CentOS', 'Scientific', 'SLC', 'Ascendos', 'CloudLinux', 'PSBM', 'OracleLinux', 'OVS', 'OEL': {
          $osfamily = 'RedHat'
        }
        'ubuntu', 'debian': {
          $osfamily = 'Debian'
        }
        'SLES', 'SLED', 'OpenSuSE', 'SuSE': {
          $osfamily = 'Suse'
        }
        'Solaris', 'Nexenta': {
          $osfamily = 'Solaris'
        }
        default: {
          $osfamily = $::operatingsystem
        }
      }
    }

This module is based on work by Dan Radez

## Usage

### pacemaker
Installs Pacemaker and corosync and creates a cluster
class {"pacemaker::corosync":
    cluster_name => "cluster_name",
    cluster_members => "192.168.122.3 192.168.122.7",
}

The pacemaker::corosync resource must be executed on each node

### Disable stonith
class {"pacemaker::stonith":
    disable => true,
}

### Add a stonith device
class {"pacemaker::stonith::ipmilan":
    address => "10.10.10.100",
    user => "admin",
    password => "admin",
}

### Add a floating ip
class {"pacemaker::resource::ip":
    ip_address => "192.168.122.223",
}

### Manage a Linux Standard Build service
class {"pacemaker::resource::lsb":
    name => "httpd",
}
