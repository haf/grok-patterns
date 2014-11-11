## Getting Started

```
git clone git://github.com/haf/grok-patterns.git
cd grok-patterns
git submodule update --init
./run
```

When you're in the box (through the script 'run'), edit the file
`confs/logstash/logstash.conf` to change the logstash config.

You can then do

```
cd /opt/logstash
bin/logstash --configtest -f /etc/logstash/conf.d
=> Configuration OK
```

To add patterns, add them in `/etc/logstash/patterns`

### Testing Locally

```
./test groks/audit-EXECVE
```

## References:

 - http://blog.jasonantman.com/2012/09/rvm-and-ruby-1-9-to-test-logstash-grok-patterns-on-fedoracentos/


## About the Patterns

### Audit

[auditd man page](http://linux.die.net/man/8/auditctl)

#### groks/auditd-EXECVE

Needs [mutate filter](https://groups.google.com/forum/#!topic/logstash-users/qmEWB780Cas) to extract parameters
