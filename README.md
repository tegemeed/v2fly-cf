# v2fly-cf

1. 注册IBM Cloud账号

（注册时地区选择中国，则分配到的免费服务器在美国达拉斯；注册时地区选择冰岛，则分配到的免费服务器在英国伦敦。这一步影响后续API网址的选择）

2. 访问 https://cloud.ibm.com/account/cloud-foundry    获得你组织的名称和空间名称

（如果是刚注册的号，组织名称是你的邮箱地址，空间名称是dev。可以不改，直接使用。顺便检查一下分配的服务器是美国还是英国。）

3. 复制你的用户名，密码。组织名称，空间名称，然后新建一个V2Ray的UUID. 

（通过V2RayN可以生成）

4. Fork本项目，在你的项目地址下添加五个Secrets：

CF_UUID_IBM

CF_USER_IBM

CF_PASSWORD_IBM

CF_ORG_IBM

CF_SPACE_IBM


从上往下依次在内容区填入UUID, 邮箱，密码，组织名称（一般为邮箱），空间名称(一般为dev)

5. 修改项目内.github/workflow里面的deploy to IBM文件。

（1）修改crontab（可选）

现在IBM限制了3天必须对服务器有操作，否则休眠。

因此，我的Crontab设置了每天UTC 20:00自动更新部署，即对于我们中国时间UTC+8 来说，是凌晨四点自动更新部署。

当然，你想改也是可以的。

（2）修改API地址(可选)

如果是美国地区的服务器，只需要添加一行空行（在哪都行），以测试workflow的结果是否正常。

如果是英国地区的服务器，需要更改cf_api为：

https://api.eu-gb.cf.cloud.ibm.com

6.进Actions区查看输出。

7 如果一切正常，去IBM云访问应用程序Url,复制下来。

去掉"https://"和最后的"/"

然后，去登录或者注册一个Cloudflare账号。

Cloudflare Workers新建一个Worker

按照IBMYes的设置即可。

附带代码：

addEventListener(

"fetch",event => {

let url=new URL(event.request.url);

url.hostname="ibmyes.us-south.cf.appdomain.cloud";

let request=new Request(url,event.request);

event. respondWith(

fetch(request)

)

}

)




8. 配置V2RayN

地址：填写你筛选后的优质Cloudflare地址

端口：443

UUID；填写你在Secrets里添加的UUID.

额外ID：64

加密方式:Auto

传输方式:ws

底层传输安全:tls

AllowSecure: false

Host: 填写Cloudflare Workers地址，不要有https://和/

Path: /



9. All Done!
