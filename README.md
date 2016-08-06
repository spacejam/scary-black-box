# :ghost: scary black box :scream:

## what?

lightweight in-memory isolated process launcher for use with fault injection
tooling for distributed systems. we need to support:

* giving a process its own hostname and filesystem
* degrade networks
* degrade name resolution
* slam disks into RO
* corrupt disks
* mess with IO
* messing with clocks
* simple configuration for the common case of starting N processes that are 
configured to know about some of each other

## why?

I want to containerize different systems quickly, so that I can focus on 
fault injection for clients.

## how?

this is pretty lightweight wrapper around systemd-nspawn, tc, echo, and mount.

## usage

we've got a database binary, `kvd`, which takes a `--peers=h1,h2,h3...`
argument with all of the other peers. we want to stand up 3 instances that 
know about each other, then start messing with it.

```
sbb -d /path/to/kvd_dir -n 3 -c './kvd --peers={{otherpeers}}'
```

this will create a tmpfs for each `-n`, copy the files in the directory
specified with `-d` into it, use systemd-nspawn to rig up a network and kick 
off the command.
