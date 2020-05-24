### Load Testing with Apache Jmeter
a simple load testing demo using jmeter &amp; elasticsearch

#### Prequisites
1. Java - https://www.java.com/en/download/
2. Homebrew - https://brew.sh/
3. Docker - https://docs.docker.com/docker-for-mac/install/
4. postman - for rest calls - you can use browser as well - https://www.postman.com/downloads/

#### Setup project repo  
`git clone https://turbokat@github.com/turbokat/load-testing-jmeter.git`

#### Download & run Elasticsearch using Docker
`docker run --name es_instance -d -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.7.0`  
`docker ps -a`. 

**Check to see if ES is up & running**  
http://localhost:9200/

#### Download & run Kibana using Docker
`docker run --name kibana_instance --link es_instance:elasticsearch -d -p 5601:5601 docker.elastic.co/kibana/kibana:7.7.0`  
`docker ps -a`  

**Check to see if Kibana is up & running**  
http://localhost:5601/

**Once Kibana is up, setup some sample data using Kibana (just follow instructions once Kibana loads)**
1. add logs data
2. setup own monitoring - not metricbeat

### Download & install jmeter
`brew list --versions jmeter`  
`brew install jmeter`  
`brew info jmeter`  

### Run the test plan
`jmeter -n -t my-first-test-plan.jmx -l test-results.jtl`  

**open the jtl file in Jmeter to see graphs**  

### get & update es docker ip into the test plan when running jmeter on docker
`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id`  

### run jmeter as a docker container - 
`docker run --name jmeter -i -v ${PWD}:${PWD} -w ${PWD} justb4/jmeter:5.1.1 $@ -n -t my-first-test-plan.jmx -l test-results.jtl -j jmeter.log`  
`sh run.sh -n -t my-first-test-plan.jmx -l test-results.jtl -j jmeter.log`  
