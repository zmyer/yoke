## Yoke

Yoke is a postgres redundancy/auto-failover solution


### Usage

Yoke has the following requirements/dependancies to run:

- a 3 server cluster consisting of a 'primary', 'secondary', and 'monitor' node
- 'primary' & 'secondary' nodes need ssh connections between each other (w/o passwords)
- 'primary' & 'secondary' nodes need rsync (or some alternative sync_command) installed
- 'primary' & 'secondary' nodes should have postgres installed under a postgres user, and in the `path`

Each node in the cluster requires it's own config.ini file with the following options (provided values are defaults):

    [config]
      advertise_ip=         # REQUIRED - the IP which this node will broadcast to other nodes
      advertise_port=4400   # the port which this node will broadcast to other ndoes
      data_dir=/data/       # the directory where postgresql was installed
      decision_timeout=10   # delay before node dicides what to do with postgresql instance
      log_level=info        # log levels available here: https://github.com/jcelliott/lumber
      peers=                # REQUIRED - the (comma delimited) IP:port combination of all other nodes that are to be in the cluster
      pg_port=5432          # the postgresql port
      role=monitor          # REQUIRED - either 'primary', 'secondary', or 'monitor' (the cluster needs exactly one of each)
      status_dir=./status/  # the directory where node status information is stored
      sync_command="rsync -a --delete {{local_dir}} {{slave_ip}}:{{slave_dir}}"

#### Startup
Once all configurations are in place, to start yoke simply run:

    ./yoke ./primary.ini

NOTE: The file can be named anything, and reside anywhere, all yoke needs is the /path/to/config.ini on startup


### Documentation

Complete documentation is available on [godoc](http://godoc.org/github.com/pagodabox-tools/yoke).


### Contributing
