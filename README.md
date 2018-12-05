# Elasticsearch, Logstash, Kibana (ELK) Docker image

Forked from (https://github.com/spujadas/elk-docker).

Changes made to this image from the origin:
- activate the TCP input logstash plugin, so that it works with [The Logstash Logback Appender](https://github.com/logstash/logstash-logback-encoder).
- change the output format back to the non-Filebeats pattern
- add a GeoIp database and activate the transformations in the input.
- Customize to allow passing in the ES_MAX_MEM parameter thru Docker environment vars
- Add the repository-s3 plugin for backups
- enable CORS for localhost to allow elasticsearch-HQ plugin to work (elasticsearch.yml)

In order to test the image, you'll want to create at least one record in the ES index to enable Kibana to load.  You can use this:
    
    sysctl -w vm.max_map_count=262144
    docker run -d -p 5601:5601 -p 5000:5000 --name elk --ulimit nofile=65536:65536 kenwdelong/elk-docker:latest
    nc -w 3 localhost 5000 < ./test/test.json
    
This will create the logstash index and allow Kibana to work.  Point your browser at port 5601.

To specify data without using a file try,

    echo '{"@timestamp": "2016-07-18T14:05:00.000+00:00", "@version": 1, "message": "this is a cmd line test" }' |  nc -w 3 localhost 5000

In order to output logstash activity to /var/log/logstash/logstash.stdout, add this line to /etc/logstash/conf.d/30-output.conf 

      stdout { codec => rubydebug }
    
## Funky Kibana Bug

If you build from this repo, when you start the container the first time, Kibana will likely crash. NodeJs needs to do some "optimizing".  If you take my image from Docker Hub, that won't happen, because I have to fix it first before I can use it. This is my procedure:

1. Build (`docker build -t elk .`) and run container as above.
1. After a couple minutes, Kibana crashes. (you can watch it on `top` or something - Kibana UID is 993 and the process is `node`)
1. Enter the container (`docker exec -it elk bash`)
1. From `/opt/kibana`, as root (those should be the defaults), issue `bin/kibana`.  This might run for a long time. For the 6.5.0 release, this crashed with an OOM on an EC2 t2.large instance. It succeeded with a t2.xlarge. Maybe changing the node heap ([here](https://github.com/wazuh/wazuh-kibana-app/issues/664) or [here](https://github.com/nreese/kibana-time-plugin/issues/30)) might help.
1. Run `chown -R kibana:kibana .`
1. Exit container
1. Push the image to Docker Hub
    1. `sudo docker login`
    1. `sudo docker commit elk kenwdelong/elk-docker:ELK-5.6.0`
    1. `sudo docker push kenwdelong/elk-docker:ELK-5.6.0`
	