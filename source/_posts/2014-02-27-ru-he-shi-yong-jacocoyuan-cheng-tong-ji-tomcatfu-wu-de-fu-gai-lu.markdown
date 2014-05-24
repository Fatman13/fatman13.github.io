---
layout: post
title: "如何使用Jacoco远程统计tomcat服务的覆盖率"
date: 2014-02-27 20:18
comments: true
categories: 
- 技术
- Jacoco
- Tomcat
---
本文将简单介绍如何使用`Jacoco`生成远程`tomcat`服务的覆盖率报告。  
(*注：使用`jacoco`打开远程服务端口，有一定安全风险。*)

<!--more-->

# 软件安装

- [Ant](http://ant.apache.org/bindownload.cgi)
- [Jacoco](http://www.eclemma.org/jacoco/)

# 远程Tomcat服务配置

- `sh shutdown.sh`先关闭`tomcat`服务。
- 修改`bin/catalina.sh`中`JAVA_OPTS`的配置。

``` sh 
# -javaagent: 的后面跟jacoco的安装路径
# includes= 选项，选择你要覆盖率的服务
# port= 选项，选择你要打开的端口
# address= 选项，tomcat服务所在机器的ip地址（如果想在跟tomcat服务同一台机器上执行ant任务的话，需要改为127.0.0.1）
JAVA_OPTS="-javaagent:/path/to/your/jacoco_0.6.4/lib/jacocoagent.jar=includes=com.baidu.*,output=tcpserver,port=8893,address=10.81.14.77"
```
- `sh startup.sh`重新启动`tomcat`服务。

# 本地Ant任务配置

- 配置`build.xml`。

``` xml
<?xml version="1.0" ?>
<project name="Lengyu" xmlns:jacoco="antlib:org.jacoco.ant" default="jacoco">
    <!--Jacoco的安装路径-->
	<property name="jacocoantPath" value="/home/work/software/jacoco_0.6.4/lib/jacocoant.jar"/>
	<!--最终生成.exec文件的路径，Jacoco就是根据这个文件生成最终的报告的-->
	<property name="jacocoexecPath" value="/home/work/local/hudson_home/workspace/wg_merchant_oc_regression/jacoco.exec"/>
    <!--生成覆盖率报告的路径-->
	<property name="reportfolderPath" value="E:/Libs/coverage_ant_task/report/"/>
	<!--远程tomcat服务的ip地址-->
	<property name="server_ip" value="10.81.14.77"/>
	<!--前面配置的远程tomcat服务打开的端口，要跟上面配置的一样-->
	<property name="server_port" value="8893"/>
	<!--源代码路径-->
	<property name="checkOrderSrcpath" value="E:/Src/ordercenter/ordercenter-biz/src/main/java/" />
	<!--.class文件路径-->
	<property name="checkOrderClasspath" value="E:/Src/ordercenter/ordercenter-biz/target/classes/com/baidu/ordercenter/service/Impl" />

	<!--让ant知道去哪儿找Jacoco-->
	<taskdef uri="antlib:org.jacoco.ant" resource="org/jacoco/ant/antlib.xml">
		<classpath path="${jacocoantPath}" />
	</taskdef>

	<!--dump任务:
		根据前面配置的ip地址，和端口号，
		访问目标tomcat服务，并生成.exec文件。-->
	<target name="dump">
	    <jacoco:dump address="${server_ip}" reset="false" destfile="${jacocoexecPath}" port="${server_port}" append="true"/>
	</target>
	
	<!--jacoco任务:
		根据前面配置的源代码路径和.class文件路径，
		根据dump后，生成的.exec文件，生成最终的html覆盖率报告。-->
	<target name="report">
		<delete dir="${reportfolderPath}" />
		<mkdir dir="${reportfolderPath}" />
		
		<jacoco:report>
			<executiondata>
				<file file="${jacocoexecPath}" />
			</executiondata>
				
			<structure name="JaCoCo Report">
				<group name="Check Order related">			
					<classfiles>
						<fileset dir="${checkOrderClasspath}" />
					</classfiles>
					<sourcefiles encoding="gbk">
						<fileset dir="${checkOrderSrcpath}" />
					</sourcefiles>
				</group>
			</structure>

			<html destdir="${reportfolderPath}" encoding="utf-8" />			
		</jacoco:report>
	</target>
</project>
```

# 生成覆盖率报告

- 执行`ant dump`。成功的话，应会有如下输出。

```
[work@st01-ecom-jn2.st01.baidu.com ant]$ ant dump
Buildfile: /home/work/local/hudson_home/workspace/wg_merchant_oc_regression/ant/build.xml

dump:
[jacoco:dump] Connecting to /10.81.14.77:8893
[jacoco:dump] Dumping execution data to /home/work/local/hudson_home/workspace/wg_merchant_oc_regression/jacoco.exec

BUILD SUCCESSFUL
Total time: 0 seconds
```

- 最后执行`ant report`。`jacoco`就会在你指定的路径生成覆盖率报告了。

# 附录

- 更多关于`Jacocoagent`以及各种`task`可以参考[官方文档](http://www.eclemma.org/jacoco/trunk/doc/ant.html)
- `Jenkins`的`Jacoco plugin`可以根据`.exec`文件直接生成覆盖率报告，并在`Jenkins`中生成图表等等。那样的话，`ant report`这个任务就没用了。
- 如有问题，可发email至[lengyu@baidu.com](mailto:lengyu@baidu.com)。
