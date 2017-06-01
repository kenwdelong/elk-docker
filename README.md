# Elasticsearch, Logstash, Kibana (ELK) Docker image

Forked from (https://github.com/spujadas/elk-docker).

Changes made to this image from the origin:
- activate the TCP input logstash plugin, so that it works with [The Logstash Logback Appender](https://github.com/logstash/logstash-logback-encoder).
- change the output format back to the non-Filebeats pattern
- add a GeoIp database and activate the transformations in the input.
- Customize to allow passing in the ES_MAX_MEM parameter thru Docker environment vars
- Add the repository-s3 plugin for backups
- enable CORS for localhost to allow elasticsearch-HQ plugin to work

In order to test the image, you'll want to create just one record in the ES index to enable Kibana to load.  You can use this:
    
    docker run -d -p 5601:5601 -p 5000:5000 --name elk kenwdelong/elk-docker:latest
    nc -w 3 localhost 5000 < ./test/test.json
    
This will create the logstash index and allow Kibana to work.  Point your browser at port 5601.

<<<<<<< HEAD
To specify data without using a file try,

    echo '{"@timestamp": "2016-07-18T14:05:00.000+00:00", "@version": 1, "message": "this is a cmd line test" }' |  nc -w 3 localhost 5000

In order to output logstash activity to /var/log/logstash/logstash.stdout, add this line to /etc/logstash/conf.d/30-output.conf 
=======
- `latest`, `540`: ELK 5.4.0.

- `532`: ELK 5.3.2.

- `531`: ELK 5.3.1.

- `530`: ELK 5.3.0.

- `522`: ELK 5.2.2.

- `521`: ELK 5.2.1.

- `520`: ELK 5.2.0.

- `512`: ELK 5.1.2.

- `511`: ELK 5.1.1.

- `502`: ELK 5.0.2.

- `es501_l501_k501`: ELK 5.0.1.

- `es500_l500_k500`: ELK 5.0.0.

- `es241_l240_k461`: Elasticsearch 2.4.1, Logstash 2.4.0, and Kibana 4.6.1.

- `es240_l240_k460`: Elasticsearch 2.4.0, Logstash 2.4.0, and Kibana 4.6.0.

- `es235_l234_k454`: Elasticsearch 2.3.5, Logstash 2.3.4, and Kibana 4.5.4.

- `es234_l234_k453`: Elasticsearch 2.3.4, Logstash 2.3.4, and Kibana 4.5.3.

- `es234_l234_k452`: Elasticsearch 2.3.4, Logstash 2.3.4, and Kibana 4.5.2.

- `es233_l232_k451`: Elasticsearch 2.3.3, Logstash 2.3.2, and Kibana 4.5.1.

- `es232_l232_k450`: Elasticsearch 2.3.2, Logstash 2.3.2, and Kibana 4.5.0.

- `es231_l231_k450`: Elasticsearch 2.3.1, Logstash 2.3.1, and Kibana 4.5.0.
 
- `es230_l230_k450`: Elasticsearch 2.3.0, Logstash 2.3.0, and Kibana 4.5.0.

- `es221_l222_k442`: Elasticsearch 2.2.1, Logstash 2.2.2, and Kibana 4.4.2.

- `es220_l222_k441`: Elasticsearch 2.2.0, Logstash 2.2.2, and Kibana 4.4.1.

- `es220_l220_k440`: Elasticsearch 2.2.0, Logstash 2.2.0, and Kibana 4.4.0.

- `E1L1K4`: Elasticsearch 1.7.3, Logstash 1.5.5, and Kibana 4.1.2.

**Note** – See the documentation page for more information on pulling specific combinations of versions of Elasticsearch, Logstash and Kibana.

### Documentation

See the [ELK Docker image documentation web page](http://elk-docker.readthedocs.io/) for complete instructions on how to use this image.

### Docker Hub

This image is hosted on Docker Hub at [https://hub.docker.com/r/sebp/elk/](https://hub.docker.com/r/sebp/elk/).

### About

Written by [Sébastien Pujadas](https://pujadas.net), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
>>>>>>> 540

      stdout { codec => rubydebug }
    
