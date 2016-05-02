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
    
    docker run -d -p 5601:5601 -p 5000:5000 --name elk kenwdelong/elk-docker:latest
    nc -w 3 localhost 5000 < ./test/test.json
    
This will create the logstash index and allow Kibana to work.  Point your browser at port 5601.
