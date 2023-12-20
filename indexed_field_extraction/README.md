## Index-time field extraction examples

### Define a new indexed field

#### transforms.conf
```conf
[netscreen-error]
REGEX =  device_id=\[\w+\](?<err_code>[^:]+)
FORMAT = err_code::"$1"
WRITE_META = true

```

#### props.conf

```conf

[testlog]
TRANSFORMS-netscreen = netscreen-error

```


#### fields.conf

```conf

[err_code]
INDEXED=true

```

--------------------------------------------------------------

### Define two new indexed fields with one regex

#### transforms.conf

```conf
[ftpd-login]
REGEX = Attempt to login by user: (.*): login (.*)\.
FORMAT = username::"$1" login_result::"$2"
WRITE_META = true

```

#### props.conf

```conf
[ftpd-log]
TRANSFORMS-login = ftpd-login
```



#### fields.conf

```conf
[username]
INDEXED=true

[login_result]
INDEXED=true

```

--------------------------------------------------------------


### Concatenate field values from event segments at index time:

#### transforms.conf

```conf 
[dnsRequest]
REGEX = UDP[^\(]+\(\d\)(\w+)\(\d\)(\w+)\(\d\)(\w+)
FORMAT = dns_requestor::$1.$2.$3 

```

#### props.conf

```conf 
[server1]
TRANSFORMS-dnsExtract = dnsRequest
```


#### fields.conf

```conf
[dns_requestor]
INDEXED = true

```
