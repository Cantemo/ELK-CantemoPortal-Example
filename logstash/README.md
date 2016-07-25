# Logstash example

1. Insert the example files into /etc/logstash/conf.d/ and restart logstash.
2. After you have restarted logstash you will need to refresh the indicies in kibana


## Portal log grok:

```
%{TIMESTAMP_ISO8601:timestamp} (?<loglocalname>portal portal) - %{DATA:class} - (?<process_name>\b.+)\[%{POSINT:process_id}] - %{WORD:loglevel} - %{GREEDYDATA:log_message}
```
