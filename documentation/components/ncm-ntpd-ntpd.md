---
layout: documentation
title: ntpd
category: documentation
subcategory: components
---
### NAME

NCM::ntpd - NCM ntpd configuration component

### SYNOPSIS

- Configure()

    This component configures the ntpd (Network Time Protocol) server to
    start at boot time, and configures the time servers from CDB. If
    anything changed in the configuration, it will restart ntpd.

- Unconfigure()
Does nothing.

### RESOURCES

- /software/components/ntpd/active : boolean

    activates/deactivates the component.

- /software/components/ntpd/servers/ : list of time servers
- /software/components/ntpd/clientnetworks : list of { net=, mask= }

    optional: clients that can use this server to synchronize. Default allows
    connections from localhost only.

### DEPENDENCIES

#### Components to be run before:

none.

#### Components to be run after:

none.

### BUGS

none known.

Thorsten Kleinwort <Thorsten.K>

### SEE ALSO

ncm-ncd(1), ntpd(1)
