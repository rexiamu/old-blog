---
title: springboot 项目使用 tomcat 部署到 linux 服务器
date: 2022-04-02 13:38:49
tags:
  - 工具
  - tomcat
  - springboot
  - java
  - linux
categories:
  - [工具使用]
  - [后端]
---

![tomcat](https://gitee.com/Rexiamu/image-hosting/raw/master/img/20220402134323.jpg)

**springboot** 项目打包 **war** 包，通过 **tomcat** 部署。 

<!-- more -->

## 打包前的配置

1. 将 **springboot** 项目打包方式改为 **war**

```xml
<!-- pom.xml-->
<packaging>war</packaging>
```

2. 多模块排除内置 **tomcat**

```xml
<!-- pom.xml-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

3. 修改文件路径

```yml
# application.yml
# 文件路径 示例（ Windows配置D:/projectName/uploadPath，Linux配置 /home/projectName/uploadPath）
profile: /home/projectName/uploadPath
```

4. 修改 **redis** 端口号

```yml
# application.yml
# redis 配置
redis:
  port: 6666
```

5. 打包

```bash
mvn clean package -Dmaven.test.skip=true
```

## tomcat 配置

1. 将 `war` 包放在 `webapps` 目录中

2. 修改 `conf\server.xml` 文件，新增项目的配置信息，多个项目可配置多个（指定不同的端口号和文件路径）

```xml
<!-- server.xml-->
<Service name="projectName">
  <Connector port="8888"
    protocol="HTTP/1.1"
    SSLEnabled="true"
    scheme="https"
    secure="true"
    keystoreFile="/usr/local/tomcat/cert/www.baidu.com.pfx"
    keystoreType="PKCS12"
    keystorePass="123456"
    clientAuth="false"
    SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"
    ciphers="TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA256" />

  <Engine defaultHost="localhost" name="projectName">
    <Realm className="org.apache.catalina.realm.LockOutRealm">
      <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
      resourceName="UserDatabase"/>
    </Realm>
    <Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="true">
      <Context path="/" docBase="projectName" reloadable="false" crossContext="true"/>
      <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
        prefix="localhost_access_log" suffix=".txt" pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      <Valve className="org.apache.catalina.valves.RemoteIpValve" remoteIpHeader="X-Forwarded-For"
        protocolHeader="X-Forwarded-Proto" protocolHeaderHttpsValue="https"/>
    </Host>
  </Engine>
</Service>
```

- `<Service name="projectName">` 项目名称
- `<Connector port="8888"` 端口号
- `keystoreFile="/usr/local/tomcat/cert/www.baidu.com.pfx"` 证书文件地址
- `keystoreType="PKCS12"` 证书类型
- `keystorePass="123456"` 证书密码
- `<Context path="/" docBase="projectName"` 文件路径与文件名称

## 重启服务

在 `bin` 目录中执行

1. 停止当前服务

```bash
./shutdown.sh
```

2. 开启服务

```bash
nohup ./startup.sh &
```

完成！

## 问题记录

1. 重启 tomcat 失败，报错 stopping Catalina ...  Connection refused

> 解决方案：
> 1. `ps aux | grep tomcat` 查看 tomcat 所占用的端口
> 2. `kill -9 进程号` 依次关闭进程 
> 3. `nohup ./startup.sh &` 重启服务

