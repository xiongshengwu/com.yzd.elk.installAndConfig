### ���밲װnginx 1.8.1 ������
1.����׼�����Ȱ�װ׼������
```
yum install gcc gcc-c++ automake pcre pcre-devel zlip zlib-devel openssl openssl-devel 
```

2.����nginx ��װ��
```
[root@localhost ~]# cd /usr/local/src/
wget http://nginx.org/download/nginx-1.8.1.tar.gz 
tar zxvf nginx-1.8.1.tar.gz 
cd nginx-1.8.1
./configure && make && make install
```

3.����Nginx
```
whereis nginx (�鿴nginx��Ŀ¼)
cd /usr/local/nginx/
sbin/nginx (����Nginx)
ps -aux|grep nginx |grep -v 'grep'
netstat -ntlp
```

### nginx�Ļ�������
```
����
[root@localhost ~]# /usr/local/nginx/sbin/nginx
ֹͣ/����
[root@localhost ~]# /usr/local/nginx/sbin/nginx -s stop(quit��reload)
����
[root@localhost ~]# /usr/local/nginx/sbin/nginx -s reload
�������
[root@localhost ~]# /usr/local/nginx/sbin/nginx -h
��֤�����ļ�
[root@localhost ~]# /usr/local/nginx/sbin/nginx -t
�����ļ�
[root@localhost ~]# vim /usr/local/nginx/conf/nginx.conf
```
### nginx�ο��ĵ�
- [Nginx Linux��ϸ��װ����̳�](https://www.cnblogs.com/taiyonghai/p/6728707.html)
- [Nginx ֮һ�����밲װnginx 1.8.1 ������](https://www.cnblogs.com/zhang-shijie/p/5294162.html)

### ʹ��nginx����kibana�����������֤
```
server {
        listen       80;
        server_name  localhost;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
		
	    auth_basic "Restricted Access";
	    auth_basic_user_file /usr/local/nginx/conf/htpasswd.users; #auth_basic_user_file��ʾ�˺������ŵ��ı���ַ 
        location /{
            proxy_http_version 1.1;
            proxy_set_header   Connection          "";
            proxy_set_header   Host             $host;
            proxy_set_header   X-Real-IP        $remote_addr;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_pass http://192.168.0.52:5601; #ת����kibana
        }
		
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
```
�˺��������ɣ�
����һ��
ͨ��http://www.matools.com/htpasswd �����ɲ���Crypt(all Unix servers)��ʽ�ܵ�����
��������
# yum install -y httpd-tools
# htpasswd -bc /usr/local/nginx/conf/htpasswd.users admin admin
# cat /usr/local/nginx/conf/htpasswd.users
```
### ʹ��nginx����kibana�����������֤-�ο��ĵ�
- [ʹ��nginx����kibana�����������֤](https://www.cnblogs.com/keithtt/p/6593866.html)
- [ElasticSearch ͨ��nginx��HTTP��֤](https://www.cnblogs.com/phpshen/p/8600582.html)
- [Nginx + http basic ���Ʒ���Elasticsearch](https://blog.csdn.net/ysl1242157902/article/details/77101109)

### HTTP-[Nginx������http��https��ͬʱ���ʷ���](https://www.cnblogs.com/doseoer/p/5663182.html)

### ����ǽ����
```
�༭����ǽ������
[root@localhost ~]# vim /etc/sysconfig/iptables
��������һ�д���
-A INPUT -p tcp -m state -- state NEW -m tcp --dport 80 -j ACCEPT
�����˳�����������ǽ
[root@localhost ~]# service iptables restart
�ο���
Nginx Linux��ϸ��װ����̳�
https://www.cnblogs.com/taiyonghai/p/6728707.html
```