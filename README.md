# Dockerized ESRI Geoportal Catalog Server and Harvester 
This project bundles all prerequisites needed to run the geoportal server and a harvester are captured in this docker composition. The docker-compose.yml creates two containers, one running Tomcat with the [geoportal-server-catalog](https://github.com/Esri/geoportal-server-catalog) and the [geoportal-harvester](https://github.com/Esri/geoportal-harvester). The second container provides the Elasticsearch server for storing and indexing the data stored in the catalog server.
This was initially based on the docker at: https://github.com/ma-ku/docker-geoportal
It has been enchanced by 
  * creating the webapplications/war files in a maven container,
  * copying the resulting artifacts into the tomcat comtainer
  * copying of cofig files into builds

## Releases and Downloads
- Version 2.6.0 of geoportal-server-catalog and geoportal-harvester.

## Installation
 
Clone the repository to your local drive. In order to build the containers and run them, use the following commands:
```bash
$ git clone https://github.com/cinergi/geoportal-server-catalog-docker.git
$ cd geoportal-server-catalog-docker/src
$ docker network create geoportal
$ docker-compose build
$ docker-compose up
```
If you have an issue with it not building, but it builds on your dev machine
```bash
$ docker-compose build --no-cache
$ docker-compose up
```

## Run the applications

* Connect to http://localhost:8080/geoportal to see the geoportal in action. 
* Connect to http://localhost:8080/harvester to see the harvester in action. 

## Requirements

* Docker

# Configuration
configurations are in config.
 I cant see my changes:
 * rebuild just the geoportal:
 ```docker-compose up --build geoportal`-no-cache``
 or maybe this keeps images up until they are repalced:
 ```docker-compose up -d --force-recreate --no-deps --build geoportal```
 
 congfig not working
 in separate window
 docker ps
 docker exec src_geoportal_1 ls /usr/local/tomcat/webapps/harvester/WEB-INF/classes/config

  docker exec src_geoportal_1 cat /usr/local/tomcat/webapps/harvester/WEB-INF/classes/config/app-security.xml
  docker exec src_geoportal_1 cat /usr/local/tomcat/webapps/harvester/WEB-INF/classes/config/authentication-simple.xml
 
 
## Kubernetes 
for production:
https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html
$ grep vm.max_map_count /etc/sysctl.conf
vm.max_map_count=262144
