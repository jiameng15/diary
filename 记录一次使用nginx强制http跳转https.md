方法一：

下面代码照搬过去就行。无需做任何修改。春哥技术博客推荐此种方法，非常简单，改完以后实时生效，不用重启服务器。

```
if ($scheme = http ) {return 301 https://$host$request_uri;}
```

![img](https://www.cgtblog.com/ueditor/php/upload/image/20180308/1520489500695737.png)

方法二：

下面代码照搬过去就行。无需做任何修改。

```
if ($server_port = 80 ) {return 301 https://$host$request_uri;}
```

![img](https://www.cgtblog.com/ueditor/php/upload/image/20180308/1520489501153107.png)

方法三：

下列代码中请注意把域名修改成自己域名。

```
if ($ssl_protocol = "") { return 301 https://$server_name$request_uri; }if ($host != www.wn789.com) { return 301 $scheme://www.wn789.com$request_uri; }
```

![img](https://www.cgtblog.com/ueditor/php/upload/image/20180308/1520489501416719.png)

方法四：

下面代码中的域名请注意修改成自己的域名，切勿完全照搬。另外还要注意的是删除原来代码中的“listen 80;”。

```
server {
 listen 80;
 server_name wn789.xin www.wn789.xin;
 rewrite ^(.*) https://www.wn789.xin$1 permanent;
 }
```

![img](https://www.cgtblog.com/ueditor/php/upload/image/20180308/1520489501379454.png)

方法五：

```
server {
 listen 80;
 server_name wn789.xin www.wn789.xin www.789wn.com 789wn.comm ;
 return 301 https://$server_name$request_uri;
 }
```

![img](https://www.cgtblog.com/ueditor/php/upload/image/20180308/1520489501828964.png)