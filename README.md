# Elasticsearch, Logstash, Kibana (ELK) Docker image

Forked from (https://github.com/spujadas/elk-docker).

Changes made to this image from the origin:
- activate the TCP input logstash plugin, so that it works with [The Logstash Logback Appender](https://github.com/logstash/logstash-logback-encoder).
- change the output format back to the non-Filebeats pattern
- add a GeoIp database and activate the transformations in the input.
- Install Marvel plugin
- Customize to allow passing in the ES_MAX_MEM parameter thru Docker environment vars


### Sanity Check

In order to test the image, you'll want to create just one record in the ES index to enable Kibana to load.  You can use this:
    
This actually registers as a parse failure, but at least it creates the logstash index and allows Kibana to work.
