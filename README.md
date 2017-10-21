# ELK-Stack with Docker (Compose)

## ElasticSearch, Logstash and Kibana aka ELK : 

- Elasticsearch for deep search and data analytics
- Logstash for centralized logging, log enrichment and parsing
- Kibana for powerful and beautiful data visualizations

## Requirements

- Docker
- Docker Compose
- RSyslog

```bash
# Debian/Ubuntu like :
(sudo) apt-get install rsyslog docker docker-compose
# Archlinux like :
(sudo) pacman -S rsyslog docker docker-compose
```

## Introduction

Official docker containers used for this project (from the Docker Hub) :
- [Elasticsearch v2.0](https://hub.docker.com/_/elasticsearch/ "Elasticsearch v2.0 sur DockerHub")
- [Logstash v2.0](https://hub.docker.com/_/logstash/ "Logstash v2.0 sur DockerHub")
- [Kibana v4.2](https://hub.docker.com/_/kibana/ "Kibana v4.2 sur DockerHub")

The version of container used is the latest stable release, if you prefer to switch to the latest version (maybe unstable)  please update "docker-compose.yml" like this example :
From : 
```yml
elasticsearch:
  image: elasticsearch:2.0
[...]
```

To : 
```yml
elasticsearch:
  image: elasticsearch:latest
[...]
```

## Getting started

### Server part (Docker containers)
```bash
git clone git@github.com:pilebones/elk-stack-docker.git
cd elk-stack-docker/
docker-compose elasticsearch
# Waiting for Elasticsearch while is not fully started
docker-compose kibana
docker-compose logstash
```
[Optionnal] You can update Logstash custom config file :
```bash
vim data/logstash/conf/logstash.conf
```

### Client part

Edit rsyslog settings :
```bash
vim /etc/rsyslog.conf
```

Add this line bellow to send all log entries to Logstash inside docker :
```bash
*.* @@0.0.0.0:25826
```
And restart rsyslog :
```bash
systemctl restart rsyslog
```

__Note :__ Any log client can push a log stream to logstash by this same way.

## Testing

### To generate log entry for testing
```bash
logger -s -p 1 "This is fake entry-log !"
```

### Check Logstash entries indexed by Elastisearch
```bash
curl -XGET "0.0.0.0:9200/logstash-*/_search"
```

