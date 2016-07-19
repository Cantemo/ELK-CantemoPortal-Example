# Logstash example

## Portal log grok:

```
%{TIMESTAMP_ISO8601:timestamp} (?<loglocalname>portal portal) - %{JAVACLASS:class} - %{WORD:process_name}\[%{POSINT:process_id}] - %{WORD:loglevel} - %{GREEDYDATA:log_message}
```
