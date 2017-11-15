### 1. elasticsearch 설정

> /config/elasticsearch.yml

주요설정들

cluster.name : ykkim-cluster
node.name :  ykkim-node

### 2. kibana.yml 

> /config/kibanay.yml 



### 3. logstash 설정만들기 

> ./bin/logstash 안에 `설정파일명.conf`를 만든다.

```groovy
## filebeats를 위한 설정 ##

input {
	beats {
		codec => "json"
		port => "5044"
	}
}

filter {
    # 불필요한 필드는 제거
	mutate {
		remove_field => [ "@version", "beat", "count", "fields", "input_type","offset","source","type","host","tags","thread_name","level_value"]
	}
    # message 필드를 끄집어 내자
	json {
		source => "message"
	}
}

output {
	stdout {
		codec => rubydebug{}
	}
	elasticsearch {
		hosts => ["127.0.0.1:9200"]
		user => "elastic"
		password => "changeme"
		index => "ykkim-index"
		document_type => "ykkim-type"
	}
}
```

logstash 실행 

> ./bin/logstash -f ykkim-beats.conf

config 수정시 자동읽기

> ./bin/logstash –f apache.config --config.reload.automatic 



### 4. filebeat 설정

> /filebeat.yml

logstash와 연결

```yaml
- input_type: log
paths:
- /Users/ykkim/workspace/elastic-log-example/logs/json/*

output.logstash:
hosts: ["localhost:5044"]
```

> /filebeat - e -c filebeat.yml

