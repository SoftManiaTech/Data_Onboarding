## Set up and use HTTP Event Collector in Splunk Web:

### Create a Token:
```bash
f71e4910-1e34-4566-8785-27f72c0c9bd8
```

### First event indexing:
```bash
curl -k https://localhost:8088/services/collector/event -H "Authorization: Splunk f71e4910-1e34-4566-8785-27f72c0c9bd8" -d '{"event": "This is First Event"}'
```

-----------------------------------------------------------------------------------------------------------


## Set up and use HTTP Event Collector with configuration files:

HTTP Event Collector (HEC) stores its settings in inputs.conf and outputs.conf.

### To set the HTTP queue size globally

Path:
```bash
/opt/splunk/etc/system/local/server.conf
```

```bash
[queue=httpInputQ]
maxSize=10MB
```

Global file is not - $SPLUNK_HOME/etc/system/defualt/, It is $SPLUNK_HOME/etc/apps/splunk_httpinput/default/inputs.conf

### Global Settings: 
```bash
[http]
disabled=1
port=8088
enableSSL=1
dedicatedIoThreads=2
index = 
maxThreads = 0
maxSockets = 0
outputgroup = 
sourcetype = 
useDeploymentServer=0
# ssl settings are similar to mgmt server
sslVersions=*,-ssl2
allowSslCompression=true
allowSslRenegotiation=true
ackIdleCleanup=true
```

### Per token Settings: 

```bash
[http://<token_name>]
connection_host = 
disabled = 
index = 
indexes = 
persistentQueueSize = 
source = 
queueSize = 
sourcetype = 
token = 
```

-----------------------------------------------------------------------------------------------------------

## Set up and use HTTP Event Collector from the CLI:

There are two syntaxes to use when you administer HEC through the CLI:

1) The syntax for all other HEC actions, such as creating, deleting, and showing tokens

	Example CLI syntax:
```bash
/opt/splunk/bin/splunk http-event-collector create new-token -uri https://localhost:8089 -description "this is a new token" -disabled 1 -index log
```

```bash
/opt/splunk/bin/splunk http-event-collector enable -name new-token -uri https://localhost:8089 -auth admin:changeme
```
2) The syntax for sending data to HEC

	Example CLI syntax:

```bash
/opt/splunk/bin/splunk http-event-collector send -uri https://localhost:8089 -token new-token {"this is the data to send"}
```
