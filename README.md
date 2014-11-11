How to test

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
