## Filter and route event data to target groups

### props.conf
```conf 
[default]
TRANSFORMS-routing=errorRouting

[syslog]
TRANSFORMS-routing=syslogRouting
```

### transforms.conf
```conf 
[errorRouting]
REGEX=error
DEST_KEY=_TCP_ROUTING
FORMAT=errorGroup

[syslogRouting]
REGEX=.
DEST_KEY=_TCP_ROUTING
FORMAT=syslogGroup
```

### outputs.conf
```conf 
[tcpout]
defaultGroup=everythingElseGroup

[tcpout:syslogGroup]
server=10.1.1.197:9996, 10.1.1.198:9997

[tcpout:errorGroup]
server=10.1.1.200:9999

[tcpout:everythingElseGroup]
server=10.1.1.250:6666
```



## Discard specific events and keep the rest

### props.conf
```conf 
[source::/var/log/messages]
TRANSFORMS-null= setnull
```

### transforms.conf
```conf 
[setnull]
REGEX = \[sshd\]
DEST_KEY = queue
FORMAT = nullQueue
```

## Keep specific events and discard the rest

### props.conf

```conf 
[source::/var/log/messages]
TRANSFORMS-set= setnull,setparsing
```

### transforms.conf
```conf 
[setnull]
REGEX = .
DEST_KEY = queue
FORMAT = nullQueue

[setparsing]
REGEX = \[sshd\]
DEST_KEY = queue
FORMAT = indexQueue
```
