# ELK CantemoPortal Example Configuration

This repo is an example configuration for setting up Cantemo Portal with ElasticSearch Logstash and Kibana (ELK stack) for logging. It is for the version 2.x of ELK and is untested with version 5.x

It uses filebeat for log shipping to the Logstash server.

## Important notes!

- This is not officially supported by Cantemo AB and intended as a resource to help setting up logging.
- This has only been tested running on a separate server from Cantemo Portal. It's not supported to run ELK on the same server as Portal at this point in time. Please see the section below about running on same server as Portal.

## Prerequisites

- Installation of Cantemo Portal (tested with 2.4.x) setup and running.
- Root access to server that Cantemo Portal is installed on.
- Ability to install filebeat on the Cantemo Portal Server.
- ELK stack installed.
- Access to the ELK stack

## Installation of FileBeat

1. Follow this guide: https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html to install filebeat.
2. Copy the configuration from the filebeat folder to `/etc/filebeat/filebeat.yml`.
3. Change the configuration in `/etc/filebeat/filebeat.yml` to point to your logstash server such as: 

    ```
    # Configure what outputs to use when sending the data collected by the beat.
    # Multiple outputs may be used.
    output:
      ### Elasticsearch as output
      elasticsearch:
        # Array of hosts to connect to.
        hosts: ["192.168.1.42:9200"]
    ```

4. If you are running a logstash implementation that requires certificates:
    1. Copy the certificate from your logstash implementation to the locate machine
    2. Change the configuration of  `/etc/filebeat/filebeat.yml` to point to the certificates:

        ```
        ...
          tls:
            # List of root certificates for HTTPS server verifications
            certificate_authorities: ["/path/to/cert/for/logstash-forwarder.crt"]
        ```

5. Load the Filebeat index template into Elasticsearch (replacing `elk:9200` with hostname and port ).
    ```
    curl -XPUT 'http://elk:9200/_template/filebeat?pretty' -d@/etc/filebeat/filebeat.template.json
    ```
6. Restart Filebeat 
    ```
    /etc/init.d/filebeat restart
    ```

## Installation of ELK.

We will not cover the complete installation of the ELK stack in this guide as there are many other guides doing so and different logging needs will necessitate different configurations. It is required that you have access to the installation and that for production use you should create the certificates that are used in your File beat configuration

### Recommended Installation Guides:

- Using Docker: https://elk-docker.readthedocs.io
- UnOfficial Guide: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04
- Official Documentation: https://www.elastic.co/guide/index.html
- 

## Running on the same server as Portal.

At this point of time we can not recommend running an ELK server stack on the same server (virtual instance) as Portal.

1. We are already running ElasticSearch on Portal for searching and it's important that the operation of that is not impacted.
2. A Cantemo Portal installation is spec'ced to be the only running server software on that server, running a complete ELK stack might impact the performance of normal Portal operations.
3. The logging can serve as a useful post-mortem of problems that have happened to the server, if the logging infrastructure goes down with system it's logging it's usefulness in this situation is negated.
