### linux awk�������
- [linux awk�������](https://www.cnblogs.com/xudong-bupt/p/3721210.html)
- [Linux������֮awk����-byArvin�Ƽ�](https://www.cnblogs.com/ginvip/p/6352157.html)
- [shell�ű���δ�json�ļ���ȡһ��ĳ��ֵ](https://blog.csdn.net/lys07962000/article/details/53792312?locationNum=15&fps=1)

ʾ��1��
```
vim json.txt 
{
    "people": [
        {
            "firstName": "Brett",
            "lastName": "McLaughlin",
            "email": "aaaa"
        },
        {
            "firstName": "Jason",
            "lastName": "Hunter",
            "email": "bbbb"
        },
        {
            "firstName": "Elliotte",
            "lastName": "Harold",
            "email": "cccc"
        }
    ]
}
==
cat json.txt 
cat json.txt | awk  '/firstName/{print$0}'| awk -F '"' '{print$4}'
```
ʾ��2(��ȡES��JSON����)��
- [linux shell awk����ⲿ������������ֵ�����](https://www.cnblogs.com/chengmo/archive/2010/10/03/1841753.html)
- [linux shell ��ȡ curl ����ֵ](https://www.v2ex.com/t/181172)

```
curl  http://192.168.0.52:9200 |awk '/cluster_name/{print$0}'|awk -F '"' '{print$4}'
clusterName=curl  http://192.168.0.52:9200 |awk '/cluster_name/{print$0}'|awk -F '"' '{print$4}'
==
#!/bin/bash
RESULT=$(curl -s http://192.168.0.52:9200)
DATA=$(echo $RESULT|awk '/cluster_name/{print$0}'|awk -F '"' '{print$4}')
echo $DATA
if ["$DATA" -eq "OK"]; then
echo "OK"
fi

```
IF ELSE����ʾ����
```
check() {
RESULT=$(curl -s http://192.168.5.100/xxx.php)
echo $RESULT

if [ "$RESULT" -eq "1111" ] ; then
echo "again"
sleep 1
check
elif [ "$RESULT" -eq "0000" ] ; then
echo "exit"
exit 0
else 
echo "error"
fi
}

check
```

### shell �С�exit0 exit1 ������
```
exit��0�����������г����˳�����

exit��1�������������е����˳�����
```