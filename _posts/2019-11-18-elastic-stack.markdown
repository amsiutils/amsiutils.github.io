---
layout: post
title:  "Installing Elastic Stack on Ubuntu"
date:   2019-11-18 00:00:00 -0400
categories: 
---

This sets up an ELK stack / Elastic Stack on Ubuntu.

#### Install

```
sudo apt install apt-transport-https
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch openjdk-8-jre-headless logstash kibana nginx openssh-server
sudo wget https://gist.githubusercontent.com/amsiutils/cf4633a74aeb02b45b717bd53747f26c/raw/2d0ddd68160f418ab45c7d8fcda00184d5545d93/elasticsearch.yml -O /etc/elasticsearch/elasticsearch.yml
sudo wget https://gist.githubusercontent.com/amsiutils/69526310cbd631a3873b633292e1698b/raw/8f6f03ed39ec4ae63ee20fa43240c9cb7ae01e04/kibana.yml -O /etc/kibana/kibana.yml
sudo wget https://gist.githubusercontent.com/amsiutils/2a13aef7c541a0e49173d7f2e9d32ff8/raw/e72fe63a7b5fc74cb449d5b48be43332f7776d31/default -O /etc/nginx/sites-available/default
sudo wget https://gist.githubusercontent.com/amsiutils/7f6522ac8d286a394a663cf1c902ee82/raw/5f7f982361de07e0f509059d28c948d402032423/02-beats-input.conf -O /etc/logstash/conf.d/02-beats-input.conf
sudo wget https://gist.githubusercontent.com/amsiutils/e603e2afe2bad181296dda70076ac8ee/raw/41c41a0f3f54e112d1b14f0954758b77ffddd8a3/50-elasticsearch-output.conf -O /etc/logstash/conf.d/50-elasticsearch-output.conf
sudo mkdir -p /etc/pki/tls/certs
sudo mkdir /etc/pki/tls/private
cd /etc/pki/tls
sudo openssl req -config /etc/ssl/openssl.cnf -x509 -days 3650 -batch -nodes -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt
```

#### Configure IP

```
sudo nano /etc/nginx/sites-available/default
```

#### Start Services

```
sudo systemctl start ssh
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl start elasticsearch.service
sudo systemctl enable elasticsearch.service
sudo systemctl start kibana.service
sudo systemctl enable kibana.service
sudo systemctl start logstash
sudo systemctl enable logstash
```

#### Troubleshooting

```
tail -f /var/log/logstash/logstash-plain.log
sudo systemctl status logstash
sudo systemctl status kibana.service
sudo systemctl status elasticsearch.service
```