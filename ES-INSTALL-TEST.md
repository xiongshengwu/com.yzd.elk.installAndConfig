## ��Ҫ����ES-�״ΰ�װ����

###1. logstash��ES��ͨ�Բ���
1.���ã�first-pipeline-stdin.conf

```
input {
    stdin {
    }
}
output {
	elasticsearch { hosts => ["192.168.0.52:9200"] }
	stdout { codec => rubydebug }    
}
```
2.��֤������

```
1.
��֤
./logstash  -f first-pipeline-stdin.conf --config.test_and_exit
2.
����
 ./logstash  -f first-pipeline-stdin.conf
```
3.������֤
```
ͨ��elasticsearch-head���в鿴����
```
###2. filebeat��logstash��ͨ�Բ���
1.ͨ��telnet 
```
telnet 192.168.0.52 9200
```
####3.���òο���es-linux-conf���ļ���
```
1.
filebeat-[pj_business].yml
2.
filebeat-[pj_front_api].yml
```