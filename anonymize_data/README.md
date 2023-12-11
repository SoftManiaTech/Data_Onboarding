## Anonymize data

### There are two ways to anonymize data with a heavy forwarder:
#### 1. Use the SEDCMD setting
#### 2. Use a regular expression (regex) transform

-------------------------------------------------------------

### Use case - 1: Anonymize data with a sed script

#### inputs.conf
```conf 
[monitor:///tmp/Data_Onboarding/anonymize_data/confidential_info.log]
sourcetype = SSN-CC-Anon
```

#### props.conf
```conf
[SSN-CC-Anon]
SEDCMD-Anon = s/ss=\d{5}(\d{4})/ss=xxxxx\1/g s/cc=(\d{4}-){3}(\d{4})/cc=xxxx-xxxx-xxxx-\2/g

``` 

-------------------------------------------------------------

### Use case - 2: Anonymize data with a regular expression transform

#### inputs.conf

```conf 
[monitor:///tmp/Data_Onboarding/anonymize_data/MyAppServer.log]
sourcetype = MyAppServer-Anon
```

#### tansforms.conf
```conf
[session-anonymizer]
REGEX = (?m)^(.*)SessionId=\w+(\w{4}[&"].*)$
FORMAT = $1SessionId=########$2
DEST_KEY = _raw

[ticket-anonymizer]
REGEX = (?m)^(.*)Ticket=\w+(\w{4}&.*)$
FORMAT = $1Ticket=########$2
DEST_KEY = _raw

```

#### props.conf
```conf
[MyAppServer-Anon]
TRANSFORMS-anonymize = session-anonymizer, ticket-anonymizer

```