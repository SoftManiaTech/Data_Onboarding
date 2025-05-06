## Monitor files and directories


###  monitoring with inputs.conf

#### Global properties 

```conf
disabled = 0
host =
index =
sourcetype =
_TCP_ROUTING = defaultGroup
host_regex = 
host_segment = 

```

#### Monitor stanza properties

```conf

[monitor:///opt/splunk/etc/apps/SplunkAdmin_RealTime_SampleData]
source = input file path
crcSalt = N/A
ignoreOlderThan = 0 (disabled)
followTail = 0
whitelist = N/A
blacklist = N/A
recursive = true

time_before_close = 3
alwaysOpenFile = N/A
followSymlink = true

```

#### Batch stanza properties

```conf
[batch://<path>]
move_policy = sinkhole

```

#### To reload inputs.conf changes 

```conf
./splunk _internal call /services/data/inputs/monitor/_reload -auth

```

-------------------------------------------------------------


### Usecases

#### 1) Enable & Disable input

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_1/data_1.log]
disabled = 1

```

#### 2) Define static host value for the input

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_2/data_2-1.log]
disabled = 0

[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_2/data_2-2.log]
disabled = 0
host = Training-Host-Machine

```

#### 3) Define static sourcetype value for the input

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_3/data_3-1.log]
disabled = 0

[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_3/data_3-2.log]
disabled = 0
sourcetype = training_sourcetype

```

#### 4) Define where to send data for each input indexer 1 or indexer 2

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_4/data_4-1.log]
disabled = 0
_TCP_ROUTING = my_indexers


[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_4/data_4-2.log]
disabled = 0
_TCP_ROUTING = indexerGroup2

```

#### 5) Extract host name from file name

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_5/host_SMCD123_messages.log]
disabled = 0


[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_5/host_SMCD456_messages.log]
disabled = 0
host_regex = host_(?<host_name>.*)_messages


[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_5/host_SMCD789_messages.log]
disabled = 0
host_regex = host_(?<host_name>.*)_messages

```

#### 6) Extract host name from file path

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_6/Host_SMCD123/data_6-1.log]
disabled = 0


[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_6/Host_SMCD456/data_6-2.log]
disabled = 0
host_segment = 5

```

#### 7) Define static source name value for the input

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_7/source_extraction/data_7-1.log]
disabled = 0


[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_7/source_extraction/data_7-2.log]
disabled = 0
source = data_7-2_log_file

```

#### 8) Reindex the same data using inputs.conf property

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_8/data_8-1.log]
disabled = 0
crcSalt = someword

```

#### 9) Ignore the data older than specified duration, only index the remaining and upcoming data 

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_9/data_9*]
disabled = 0
ignoreOlderThan = 30d

```

#### 10) Index only the new records, not the old data.

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_10/data_10-1.log]
disabled = 0
followTail = 1

```

#### 11) From the particular folder, index only Log files & CSV files

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_11/*]
disabled = 0
whitelist = \.log$|\.csv$

```

#### 12) From the particular folder, don't index TXT files & CSV files 

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_12/*]
disabled = 0
blacklist =  \.(?:txt|csv)$

[WinEventLog:Security]
blacklist1 = EventCode = "4662" Message = "Account Name:\s+(example account)"

```

#### 13) Enable or Disable recursive monitoring

```conf
[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_13/with_recursive/]
disabled = 0

[monitor:///tmp/Data_Onboarding/monitor_usecases/usecase_13/without_recursive/]
disabled = 0
recursive = false

```

#### 14) Index the data from particualr folder & delete it as soon as the indexing/forwarding is done

```conf
[batch://tmp/Data_Onboarding/monitor_usecases/usecase_14/batchdata/*]
disabled = 0
move_policy = sinkhole

```
