# Elasticsearch, Logstash, Kibana (ELK) Docker image

Forked from (https://github.com/spujadas/elk-docker).

Changes made to this image from the origin:
- activate the TCP input logstash plugin, so that it works with [The Logstash Logback Appender](https://github.com/logstash/logstash-logback-encoder).
- DISABLE ECS compatibility. This was done to preserve compatibility with our previous schema. To re-enable, edit 01-lumberjack-input.conf and 30-output.conf
- change the output format back to the non-Filebeats pattern
- add a GeoIp database and activate the transformations in the input.
- Customize to allow passing in the ES\_MAX\_MEM parameter thru Docker environment vars
- Add the repository-s3 plugin for backups
- enable CORS for localhost to allow elasticsearch-HQ plugin to work (elasticsearch.yml)

In order to test the image, you'll want to create at least one record in the ES index to enable Kibana to load.  You can use this:
    
    sysctl -w vm.max_map_count=262144
    docker run -d -p 5601:5601 -p 5000:5000 --name elk --ulimit nofile=65536:65536 kenwdelong/elk-docker:latest
    nc -w 3 localhost 5000 < ./test/test.json
    
This will create the logstash index and allow Kibana to work.  Point your browser at port 5601.

To specify data without using a file, try (fix the timestamp):

    echo '{"@timestamp": "2016-07-18T14:05:00.000+00:00", "@version": 1, "message": "this is a cmd line test" }' |  nc -w 3 localhost 5000

In order to output logstash activity to /var/log/logstash/logstash.stdout, add this line to /etc/logstash/conf.d/30-output.conf 

      stdout { codec => rubydebug }
    
Written by [Sébastien Pujadas](https://pujadas.net), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
    