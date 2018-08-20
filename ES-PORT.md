### ELK端口
服务名       | 端口           | Cool  
 ------------- |:-------------:| -----:
elasticsearch      | 9200 | HTTP API
elasticsearch      | 9300 | TCP
elasticsearch-head      | 9100 | TCP
kibana      | 5601 | TCP
logstash      | 5044 | TCP

### ELK-URL地址
服务名       | URL地址           | Cool  
 ------------- |:-------------:| -----:
elasticsearch      | 9200 | 
elasticsearch-head | http://192.168.0.52:9100/ | 
kibana      | http://192.168.0.52:5601/app/kibana | 

### ELK-程序部署地址
- /usr/es/(安装启动程序)
- /logs/(程序日志目录)

### ELK-用户规化
服务名       | 启动用户名          | Cool  
 ---------- |:-------------:| -----:
elasticsearch     |elsearch | 
elasticsearch-head      | root | 
kibana      | root | 
logstash      | root |

### ES-elasticsearch目录规化 
目录       | 地址          |  
 ---------- |:-------------:
软件目录     |/usr/es/elasticsearch-5.6.10 | 
数据目录     | /data/es/data | 
日志目录     | /data/es/logs | 
