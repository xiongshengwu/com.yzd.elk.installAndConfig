----
### ͨ��ESʵ�ּ�������
- �ο����룺

```
�ϰ汾��ES
https://github.com/yaozd/MyNote/blob/master/framework/src/main/java/elasticsearch/elasticsearchs_233_demo
https://github.com/yaozd/MyNote/tree/master/framework/src/main/java/elasticsearch
�°汾��ES-5.X
https://github.com/yaozd/MyNote/tree/master/elastic5/src/main/java/com/gourd/erwa/elastic5/demo
```
### Elasticsearch�Ż�
- �ο����룺[��������η�������32����ʵʱ��־��](http://www.open-open.com/lib/view/open1437018728240.html)

```

������������Elasticsearch�Ż���Hardware Level�������ǵ�ʱ�õ�����û��ѡ����أ�ֻ�����˳��̣߳�System Level���Ż���ر�swap������max open files�ȣ�App Level���Ż���Java���л����汾��ѡ��ES_HEAP_SIZE�����ã��޸�bulk index��queue size�ȣ����⻹������Ĭ�ϵ�index template��Ŀ���Ǹ���Ĭ�ϵ�shard��replica������string��Ϊnot_analyzed������doc_values��Ӧ�� elasticsearch����OOM����ϸ���Ż����ݼ� Elasticsearch Optimization Checklist�� 
```
```
1�� elasticsearch ����JVM Heap High Usage�� > 90% ���� 

�ܳ�һ��ʱ�䣬���Ƕ���Ӧ��JVM Heap High Usage�������˵�������Old GC�����࣬ʱ�䳤��es�ڵ�Ƶ���˳���Ⱥ��������Ⱥ����ֹͣ��Ӧ���������ǵ���Ҫ�����ǿ���doc_values��[����queryִ��ʱռ�õ�JVM Heap size��analyzed stringֻ������query��������facets����aggs��]����close �û�����Ҫ��index�� 
ע��facets����aggs��ָ�������ѯ��ۺϲ�ѯ-byArvin 20180820-1111

2�� Elasticsearch Query DSL��Facets��Aggsѧϰ���� 

����Ϊ�˿�����ʹ��SQLִ��ES Query�Ĳ����һ���̶��ϼ����˽����ż������Ǹ�����ѧϰ���ǵĽ����ǹ۲�Kibana��Request Body������Marvel��Senese����������Զ����Query��Facets��Aggs�Ĺ��ܡ�������õ�query��query string query����õ�aggs��Terms��Date Histogram������Ӧ���󲿷�����
```

### DSL�ĵ��� | ����sqlת��Elasticsearch DSL�������
```
https://blog.csdn.net/laoyang360/article/details/78556221
1��sql���ת��DSL����Щ������
����һ���������� NLP���忪����Elasticsearch-sql; 
2.X��װ����5.Xû���ٰ�װ�� 
===
����������������ElasticHQ���Զ�ת��ģ�飺 
����һ����������Github���������Կ�����sqlתDSL���߶Լ򵥵�sql���ɵ�DSL���׼ȷ�������ڸ��ӵ�sql���ɵĲ�һ����ȷ����������ʾ��
��������ͽ�����ɡ�

```
### Elastic HQ���-360��ҵ��ȫ�����ն˰�ȫ�ӹ�˾��(Ŀ¼��/usr/es/)-byArvin�Ƽ�
```
https://github.com/360EntSecGroup-Skylar/ElasticHD
Elasticsearch ���ӻ�DashBoard, ֧��Es��ء�ʵʱ������Index template����滻�޸ģ������б���Ϣ�鿴�� SQL converts to DSL��
���ص�ַ����Ŀǰ�ٶ����б��ݣ�
https://github.com/360EntSecGroup-Skylar/ElasticHD/releases/
Ŀǰʹ�õ���-�汾1.4.1
������
Linux and MacOS use ElasticHD
Step1: Download the corresponding elasticHD version��unzip xxx_elasticHd_xxx.zip
Step2: chmod 0777 ElasticHD
Step3: exec elastichd ./ElasticHD -p 127.0.0.1:9800 
windows
Step1: Download the corresponding elasticHD version��Double click zip package to unzip
Step2: exec elastichd ./ElasticHD -p 127.0.0.1:9800 
3.Դ�������Source code compilation
git clone https://github.com/360EntSecGroup-Skylar/ElasticHD
cd ElasticHD
npm install
npm run build
cd ./main
statik -src=../dist
# go build
GO_ENABLED=0 GOOS=windows GOARCH=amd64  go build -o elasticHD.exe github.com/elasticHD/main
4.Docker Quick Start:
docker run -p 9200:9200 -d --name elasticsearch elasticsearch
docker run -p 9800:9800 -d --link elasticsearch:demo containerize/elastichd
Open http://localhost:9800 in Browser
```
#### Elastic HD(-360��ҵ��ȫ�����ն˰�ȫ�ӹ�˾)��װ������
```
����һ��
exec: "xdg-open": executable file not found in $PATH
https://blog.csdn.net/qq_35719950/article/details/80935677
û�а�װxdg-open
��װxdg-utils֮��xdg-open����Ϳ���ʹ����.
yum install xdg-utils
�������
xdg-open: no method available for opening 'http://127.0.0.1:9800'
�ѡ�./ElasticHD -p 127.0.0.1:9800 ����Ϊ��./ElasticHD -p 192.168.0.28:9800 ��
�Ϳ���ͨ�����������http://192.168.0.28:9800/
ע�����ֻϣ��ֻ�б�������ʹ�ã�127.0.0.1
���ϣ����������ͬ�����Է���ʹ�ã�0.0.0.0 �򱾻�IP��ַ


```
### Elastic HQ���(Ŀ¼��/usr/es/)-byArvin���Ƽ�ʹ��
```
ElasticSearch5.6.3 ��װ����
https://blog.csdn.net/yqh5566/article/details/78558977
1. ����
# wget https://github.com/royrusso/elasticsearch-HQ/archive/master.zip
2. ��ѹ
# unzip elasticsearch-HQ-master.zip
3. ��װ
# cd elasticsearch-HQ-master
4. ����
���¶�ѡһ�� 
# python -m SimpleHTTPServer��ǰ̨������ 
# nohup python -m SimpleHTTPServer > elasticsearch-HQ.file 2>&1 &����̨������
5. ��֤�Ƿ�װ�ɹ�
����http://192.168.102.30:8000/#cluster 
```
### IK�ִʲ��(Ŀ¼��/usr/es/)
```
IK�ִʲ��
ElasticSearch5.6.3 ��װ����
https://blog.csdn.net/yqh5566/article/details/78558977
ÿ̨��������Ҫ��װ 
�Բ����ʽ��װ�� 
# cd elasticsearch-5.6.3 
# ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v5.6.3/elasticsearch-analysis-ik-5.6.3.zip 
����: 
```
### ElasticSearch5.2.2 ��������
- [ElasticSearch5.2.2 ��������ͼ�Ⱥ�������](https://blog.csdn.net/WuLex/article/details/71194653)
- [ElasticSearch DSL�ṹ��һЩ˵��](https://blog.csdn.net/u011031430/article/month/2018/03)
- [Elasticsearch DSL �����﷨����](http://www.youmeek.com/elasticsearch-dsl/)
- [DSL�ĵ��� | ����sqlת��Elasticsearch DSL�������](https://blog.csdn.net/laoyang360/article/details/78556221)

### �ο��ĵ�
- ./reference/Elasticsearch��DSL_2018_8_20.html

