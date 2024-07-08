ELK Stack에서 logstash가 로그를 수집할 때, TCP 5000번을 통해 로그를 수집한다. 

데이터의 종류와 상관 없이 모든 log를 수집할 수 있는 concept는 좋지만, 
이 경우에 logstash가 죽으면 어떤 방법으로 backup 할 수 있는가?

filebeat로 우선적으로 file에 기록 후, 지속적으로 logstash에 전송하는 방식이 있다고 한다.
그렇다면 elastic search를 사용하기 위해 두 중간 다리를 거쳐야 하는 것인가..?