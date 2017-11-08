### 1. elasticsearch.yml

주요설정들

cluster.name : ykkim-cluster
node.name :  ykkim-node

### 2. kibana.yml 





### 3. logstash 설정만들기 

./bin/logstash 안에 `설정파일명.conf`를 만든다.

```groovy
## filebeats를 위한 설정 ##

input {
	beats {
		codec => "json"
		port => "5044"
	}
}

filter {
	mutate {
		remove_field => [ "@version", "@timestamp", "beat", "count", "fields", "input_type","offset","source","type","host","tags" ]
	}
}

output {
	stdout {
		codec => rubydebug{}
	}
	elasticsearch {
		host => ["127.0.0.1:9200"]
		user => "elastic"
		password => "changeme"
		index => "ykkim-index"
		document_type => "ykkim-type"
	}
}
```

logstash 실행 

> ./bin/logstash -f ykkim-beats.conf

