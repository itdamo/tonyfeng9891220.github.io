

## 环境准备 sdkman java gradle springboot
install sdkman and using it to install that needed (java\gradle\springboot)

```sh
curl -s "https://get.sdkman.io" | bash
source "/Users/fqc/.sdkman/bin/sdkman-init.sh"
sdk install java
sdk install gradle
sdk install springboot
```

![-w450](media-1/15055715565156.jpg)

注意：开启了科学上网(翻墙)代理
![-w450](media/15055778486302.jpg)


![-w450](media/15055780034539.jpg)


## install docker

```sh
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh /dev/stdin 1.12.3
```

docker 加速器
```sh
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://bbfa5e62.m.daocloud.io
```

mac版本
![](media/15060037423768.jpg)


## install sublime3 and VinTage和VinTageEx plugin

## new WebApp.groovy  && spring run

由于安装了spring-cli，所以可以直接命令行`spring run springboot-app-file`。其中springboot-app只需要是springboot的文件即可，不论是java版本还是groovy版本，都可直接运行(是需要下载依赖的)。

### sdk 切换

```sh
sdk default scala 2.11.6
```

### groovy版本

```groovy
@RestController
class WebApp{
	@RequestMapping("/")
	String greeting(){
		"greeting from groovy"
	}
}
```

just run `spring run WebApp.groovy`
![-w450](media/15055783505421.jpg)



### java版本

```java
package springbootdemo 
import org.springframework.boot.SpringApplication; 
import org.springframework.boot.autoconfigure.SpringBootApplication; 
import org.springframework.web.bind.annotation.RequestMapping; 
import org.springframework.web.bind.annotation.RestController; 

@RestController
@SpringBootApplication
public class WebApp2{
	@RequestMapping("/")
	public String greetings(){
		return "Greetings from spring boot";
	}
	public static void main(String[] args) {
		SpringApplication.run(WebApp2.class,args)
	}
}
```

just run `spring run WebApp2.java`

notice: just create file and run. the spring-cli will download the dependency. until now, it has no any config file . even pom,gradle script or properties. just one file groovy or java.

![-w450](media/15055782334167.jpg)

## 使用gradle创建springboot工程
### config build.gradle

```gradle
buildscript {

	ext {
		springBootVersion = '1.5.7.RELEASE'
	}

	dependencies {
		classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
	}

}

apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'spring-boot'

jar {
	baseName = 'myapp'
	version = '0.0.1-SNAPSHOT'
}

allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public'}
        maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
    }
}

dependencies {
	compile('org.springframework.boot:spring-boot-starter-web')
	compile("org.springframework.boot:spring-boot-starter-test")
}
```

注意spring cli 的方式不要和gradle配置文件混淆，因为没有关系
`spring init -dweb --build gradle config-server  --group="com.micro"`
**--build gradle** 只是指明创建类型为gradle方式。 
config-server 为工程名称
**--group**  为包名
**--mainClassName** 别瞎猜

```sh
☁  events_microservices [master] ⚡ spring help init                                                                                                                                [master|]
spring init - Initialize a new project using Spring Initializr (start.spring.io)

usage: spring init [options] [location]

Option              Description
------              -----------
-a, --artifactId    Project coordinates; infer archive
                      name (for example 'test')
-b, --boot-version  Spring Boot version (for example
                      '1.2.0.RELEASE')
--build             Build system to use (for example
                      'maven' or 'gradle') (default: maven)
-d, --dependencies  Comma-separated list of dependency
                      identifiers to include in the
                      generated project
--description       Project description
-f, --force         Force overwrite of existing files
--format            Format of the generated content (for
                      example 'build' for a build file,
                      'project' for a project archive)
                      (default: project)
-g, --groupId       Project coordinates (for example 'org.
                      test')
-j, --java-version  Language level (for example '1.8')
-l, --language      Programming language  (for example
                      'java')
-n, --name          Project name; infer application name
-p, --packaging     Project packaging (for example 'jar')
--package-name      Package name
-t, --type          Project type. Not normally needed if
                      you use --build and/or --format.
                      Check the capabilities of the
                      service (--list) for more details
--target            URL of the service to use (default:
                      https://start.spring.io)
-v, --version       Project version (for example '0.0.1-
                      SNAPSHOT')
-x, --extract       Extract the project archive. Inferred
                      if a location is specified without
                      an extension

examples:

    To list all the capabilities of the service:
        $ spring init --list

    To creates a default project:
        $ spring init

    To create a web my-app.zip:
        $ spring init -d=web my-app.zip

    To create a web/data-jpa gradle project unpacked:
        $ spring init -d=web,jpa --build=gradle my-dir

```


`spring init -dweb --build gradle config-server  --group="com.micro" --name="ConfigServerApplication"`

```sh
☁  events_microservices [master] ⚡ spring init -dweb --build gradle config-server  --group="com.micro" --name="ConfigServerApplication"                                            [master|]
Using service at https://start.spring.io
Project extracted to '/Users/fqc/git-work/events_microservices/config-server'
```

![](media/15062315861437.jpg)


- https://spring.io/guides/gs/multi-module/
- https://docs.spring.io/spring-boot/docs/current/reference/html/cli-using-the-cli.html

### 配置gradle仓库国内地址
[repository server](https://xiexianbin.cn/java/2016/12/18/change-gradle-maven-repo-url)

```sh
allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public'}
        maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}
    }
}
```

### spring boot cli方式执行创建

`spring init --list` 列出spring的生态环境项目
`spring init -dweb --build gradle myapp` 使用gradle脚本和web依赖创建spring工程。
`spring init -dweb --build gradle config-server  --group="com.micro" --name="ConfigServerApplication"`
## 运行gradle springboot项目

有两种方式  

1.  在gradle中指定springboot插件可以使用功能`bootRun`
2.  使用fat jar的方式 `gradle clean build && java -jar ...` ,`gradle clean build -x test`可以忽略测试(测试构建很耗时)

![](media/15060577341599.jpg)


- gradle
    - `./gradlew bootRun (project level)`使用当前的gradle可以统一约束团队的gradle版本
    - `gradlew bootRun  (global level or youself level)`

其中gradle文件夹下的gradle-wrapper.proerties中的版本可以控制，团队成员下载好代码，直接`./gradlew bootRun`将会自动下载对应版本的gradle。
![](media/15059124417310.jpg)


![](media/15059124190554.jpg)


![](media/15059125561541.jpg)


- fat jar

```sh
gradle clean build 
java -jar ...
```

![](media/15059896153868.jpg)

![](media/15059896388614.jpg)

## import to IDEA
![](media/15055806686446.jpg)

## @SpringBootApplication注解的作用
![](media/15055815175807.jpg)

springbootApplication注解首先有标注的作用
也就是说将test相关的代码注释掉，这里就可以将该注解去除掉。
![-w450](media/15055816844477.jpg)

![-w450](media/15055816642410.jpg)


![](media/15059140305572.jpg)

## 配置文件

注意两点`---`和层级关系，错一个都不行
**key: value 中间需要有空格隔开**
```yml
spring:
  profiles: dev
server:
  port: 9999

---
spring:
  profiles:
    active: prod
server:
  port: 8888

---
spring:
  profiles: test
server:
  port: 7777
```

## 开启actuator插件 （审计，健康检查，监控等产品就绪时需要的功能）

```sh
compile('org.springframework.boot:spring-boot-starter-actuator')
```

![](media/15059175048892.jpg)

当访问`/env`的时候打印出该日志信息。
![](media/15059174887180.jpg)


### management.security.enabled
很多springboot提供的系统级别api默认是开启权限验证的。开发时可以先关闭掉
`management.security.enabled = false`

## 日志配置

```sh
compile('org.springframework.boot:spring-boot-starter-logging')
```
![](media/15059191832278.jpg)

[Spring Boot 日志配置方法(超详细)](http://www.jb51.net/article/118849.htm)

[SpringBoot干货系列（七）：默认日志logback配置解析](http://www.qingpingshan.com/rjbc/java/246216.html)

## dockerfile

![](media/15059594855189.jpg)

注意以下两种方式都可以
![](media/15059861805804.jpg)



![](media/15059597671820.jpg)

![](media/15059598107300.jpg)

通常为了降低单元测试或行为测试的隔离性，比如一个服务需要底层基础设施服务，如mongodb服务等，注入一些数据，不依赖别人环境，同时不需要访问其他环境。**docker-compose所做的就是将很多container依赖组装起来。**

![](media/15059600165524.jpg)

注意 **一些配置最好能在docker-compose中配置，不用修改源码，更加方便**。

![](media/15059631734587.jpg)

![](media/15059632670779.jpg)

## 文档的重要性

拿不准的，最好先看文档。 `-SPRING_PROFILES_ACTIVE:prod`和`-SPRING_PROFILES_ACTIVE=prod`实验多少次都不会成功。 

```sh
gradle clean build && java -jar build/libs/myapp-0.0.1-SNAPSHOT.jar -SPRING_PROFILES_ACTIVE:prod
```

![](media/15059607665655.jpg)


```sh
gradle clean build && java -jar -Dspring.profiles.active=prod build/libs/myapp-0.0.1-SNAPSHOT.jar                                                             
```
注意: 一定要按照顺序来， 先指定profile，然后指定jar。相反则不会起效。
![](media/15059610545147.jpg)
[springboot属性配置官方手册](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html)

## 编写微服务
### 编写Dockerfile文件

实际的Dockerfile

```sh
FROM java:8

RUN mkdir /app
WORKDIR /app

ADD build/libs/myapp-0.0.1-SNAPSHOT.jar /app
RUN ln -s /app/myapp-0.0.1-SNAPSHOT.jar /app/myapp-latest.jar

EXPOSE 9999
CMD ["java","-jar","/app/myapp-latest.jar"]

# CMD java -jar /app/myapp-latest.jar
# RUN java -jar /app/myapp-latest.jar  永远不会运行成功
```

**RUN指定的命令是在镜像构建过程中执行的，CMD指定的命令是在容器运行中执行的**。

### 构建镜像

注意切换到有Dockerfile的目录
```sh
docker build -t myapp:myapp1.0 .
```
### 设置开机重启docker容器服务 --restart=always


```sh
docker run -d -p 9997:9999 --restart=always myapp:myapp
```
![](media/15059850979540.jpg)


## 构建spring data rest服务
![](media/15059877295096.jpg)

![](media/15059882809797.jpg)

## 将json文件导入mongodb

![](media/15059987188294.jpg)

![](media/15059985499292.jpg)

![](media/15059987022434.jpg)

注意先在本地跑起来没问题，再放入docker执行。

![](media/15060000397728.jpg)

###hal browser
add 

```sh

```
![](media/15060000748108.jpg)


## gradle docker plugin

[使用gradle生成Spring Boot应用的Docker Image](http://www.jdon.com/dl/best/docker-containers-with-gradle-in-4-steps.html)

![](media/15060119725743.jpg)


![](media/15060128201172.jpg)
运行成功！

但是面临一个尴尬的问题，镜像竟然如此之大...
而在之前的myapp也是很大...
![](media/15060128835618.jpg)


[Alpine Linux，一个只有5M的Docker镜像](http://www.infoq.com/cn/news/2016/01/Alpine-Linux-5M-Docker)
[Docker官方镜像将会使用Alpine Linux替换Ubuntu](http://dockone.io/article/1014)

其原因在于
from aglover/java8-pier
![](media/15060471661275.jpg)
![](media/15060471566798.jpg)


[Smaller Java images with Alpine Linux](https://developer.atlassian.com/blog/2015/08/minimal-java-docker-containers/)

[Spring Boot with Docker](https://spring.io/guides/gs/spring-boot-docker/)

## lstat .. :no such file or directory

![](media/15060566547546.jpg)

该问题在于上下文的构建路径。调整下即可。
比如 报错就是build/libs...找不到。 之前使用docker build .(当前路径的Dockerfile)。
而使用了gradle docker plugin之后
![](media/15060567925471.jpg)
构建的上下文路径就变了 是build目录下了

![](media/15060567114112.jpg)

修改路径后构建成功！而且使用了Dockerfile中的基础镜像，小了将近一半，从1.03G到600M，灵活。
![](media/15060568616110.jpg)


接着可以进行再瘦身，寻找apline docker java
[Lean, Mean, Java Virtual Machine: Making Your Docker 7x Lighter With Alpine Linux](http://blog.takipi.com/lean-mean-java-virtual-machine-making-your-docker-7x-lighter-with-alpine-linux/)

实际上jar包就19.8m约20m ，然后java8的基础镜像为643m 整个的服务镜像为663m很合理
大部分就是基础镜像占用了绝大部分的空间大小。

[java OFFICIAL REPOSITORY](https://hub.docker.com/_/java/) 

![](media/15060574994175.jpg)


![](media/15060579380945.jpg)
java:alpine本身是145m，加上工程19.8m，165m合理。 
![](media/15060579806803.jpg)

再次进化，脚本中没法添加忽略测试(`depondsOn: build`中没法再加`-x test`了，对groovy语言不熟，不确定)，
![](media/15060584967874.jpg)

那就在命令行执行的时候加上，果然也奏效
![](media/15060584287758.jpg)


`docker run -it $image /bin/sh `进入到容器
![](media/15060586814594.jpg)

可以看启动日志 交互的方式. 可以不关闭服务而退出的方式是:`ctrl+q+P`
![](media/15060587852183.jpg)

完美！！ 注意端口映射哈！！
![](media/15060589784104.jpg)

### docker use spring profile
![](media/15060639715740.jpg)


## docker compose up
![](media/15060701341314.jpg)

因为gradle插件配置的和Dockerfile其有冲突。 其中gradle插件的doFirst可以忽略

![](media/15060702621185.jpg)

![](media/15060702759798.jpg)


注释掉之后
![](media/15060705861310.jpg)

解决方案：
因为gradle插件的执行上下文为 build/docker。它将文件拷贝到了build/docker/下，jar在其target下，而Dockerfile也单独拷贝了一份。因此，docker-compose文件不能够在该项目当前路径上下文`build: .`，而应改为`build/docker`

![](media/15060731564430.jpg)



![](media/15060732883721.jpg)

注意下面的dockerFile中的port指定7777,那么上图中的ports:要为**9999:7777**
并且注意配置的重叠加载 在docker-compose中指定了使用`docker(只是一个命名)文件`，而默认项中的context-path是有效的


![](media/15076012030074.jpg)
图为application.yml. 指定的应用程序

## docker-compose 一键构建打包启动

```sh
gradle clean build -x test && gradle buildDocker -x test && docker-compose up
```

![](media/15060737527555.jpg)

![](media/15060733934968.jpg)


![](media/15060736281940.jpg)


访问也ok
![](media/15060734534235.jpg)

### 向mongo容器导入测试数据

```sh
#!/usr/bin/env bash

SEED_FILE_NAME='events-with-id.json'

CONTAINER_ID=$(docker ps | awk '/_mongo/ {print $1}')
echo $CONTAINER_ID
echo $(dirname $0)/

docker cp $(dirname $0)/$SEED_FILE_NAME $CONTAINER_ID:/data
docker exec $CONTAINER_ID mongoimport --db test --collection event --type json --file /data/$SEED_FILE_NAME
```
![](media/15060749900371.jpg)

刷新页面，成功看到测试数据。

![](media/15060750474726.jpg)


查看导入数据在容器内的位置
![](media/15060753299723.jpg)

## 其他资料
[Crafting perfect Java Docker build flow](https://codefresh.io/blog/java_docker_pipeline/)

## gradle 构建时有时会卡死


![](media/15075990340273.jpg)


[解决方案 关闭掉org.gradle.daemon=false](https://docs.gradle.org/current/userguide/gradle_daemon.html)


```
修改或新建 ~/.gradle/gradle.properties
添加一行 org.gradle.daemon=false
```
![](media/15075990049700.jpg)




## spring boot profile with docker
http://www.littlebigextra.com/use-spring-profiles-docker-containers/

