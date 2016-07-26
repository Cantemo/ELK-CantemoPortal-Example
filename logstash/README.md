# Logstash example

1. Insert the example files into /etc/logstash/conf.d/ and restart logstash.
2. If needed insert the patterns in to /opt/logstash/patterns/ but you might find that they are already there.
3. After you have restarted logstash you will need to refresh the indicies in kibana

## Portal log grok:

```
%{TIMESTAMP_ISO8601:timestamp} (?<loglocalname>portal portal) - %{DATA:class} - (?<process_name>\b.+)\[%{POSINT:process_id}] - %{WORD:loglevel} - %{GREEDYDATA:log_message}
```

## Nginx log grok:

This is based upon Portal 2.4.* with both response times and without response times. 
