*x-pack를 설치하기 위해서는 elasticsearch, kibana는 종료상태이어야 한다.*

### 1. elasticsearch에 설치 

> ./bin/elasticsearch-plugin install x-pack 

###2. kibana에 설치 

> ./bin/kibana-plugin install x-pack 

### 3. kibana에 접속

> http://localhost:5601/

> elastic / changeme 로 로그인

elasticsearch, kibana를 구동한 후 접속한 다음 패스워드를 변경한다. 