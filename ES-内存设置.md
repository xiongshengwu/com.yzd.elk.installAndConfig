### ElasticSearch���ڴ�����
- logstash��elasticsearch�ڴ�����

```
��ֵ�Ĵ�С�뵱ǰ�����Ŀ�ʹ����Դ�й�
��������
config->jvm.options
==
#-Xms1g
#-Xmx1g
-Xms512m
-Xmx512m
```
- [�����ڴ�ʹ��](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_limiting_memory_usage.html)

```
ѡ��Ѵ�С��Choosing a Heap Size��
������ Elasticsearch �Ѵ�Сʱ��Ҫͨ�� $ES_HEAP_SIZE ��������Ӧ����������
1.
��Ҫ�������� RAM �� 50%
Lucene �ܺܺ������ļ�ϵͳ�Ļ��棬����ͨ��ϵͳ�ں˹���ġ����û���㹻���ļ�ϵͳ����ռ䣬���ܻ��ܵ�Ӱ�졣 ���⣬ר���ڶѵ��ڴ�Խ����ζ����������ʹ�� doc values ���ֶ��ڴ�Խ�١�
2.
��Ҫ���� 32 GB
����Ѵ�СС�� 32 GB��JVM ��������ָ��ѹ��������Դ�󽵵��ڴ��ʹ�ã�ÿ��ָ�� 4 �ֽڶ����� 8 �ֽڡ�
```
- [���ڴ�:��С�ͽ���](https://www.elastic.co/guide/cn/elasticsearch/guide/current/heap-sizing.html)

```
���������ַ�ʽ�޸� Elasticsearch �Ķ��ڴ档��򵥵�һ����������ָ�� ES_HEAP_SIZE �����������������������ʱ����ȡ�������������Ӧ�����öѵĴ�С�� ���磬������������������������

export ES_HEAP_SIZE=10g
���⣬��Ҳ����ͨ�������в�������ʽ���ڳ���������ʱ����ڴ��С���ݸ��������������������򵥵Ļ���

./bin/elasticsearch -Xmx10g -Xms10g 


ȷ�����ڴ���Сֵ�� Xms �������ֵ�� Xmx ���Ĵ�С����ͬ�ģ���ֹ����������ʱ�ı���ڴ��С�� ����һ���ܺ�ϵͳ��Դ�Ĺ��̡�

ͨ����˵������ ES_HEAP_SIZE ������������ֱ��д -Xmx -Xms ����һ�㡣
```
- [��Ҫ������Щ���ã�](https://www.elastic.co/guide/cn/elasticsearch/guide/current/dont-touch-these-settings.html)

```
1.
��������
Ŀǰ����ʹ��CMS��Ϊ���գ�G1��ʱ���ǲ��ȶ���byArvin-20180917-1423��
2.
�̳߳�
```