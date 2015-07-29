#安装部署手册

# 1  概述 #

## 1.1  硬件环境 ##

本项目至少需要一台计算机来满足项目的部署需求。

## 1.2  软件环境 ##

# 2 安装步骤

## 2.1 基础环境搭建

本项目依赖`tomcat7-admin`环境和`maven`项目管理等软件进行安装部署。可运行main目录下的`base_environment_install.sh`文件进行快速的基础环境搭建。
在正式部署本项目前，需根据具体的架构对项目进行设置。

## 2.2 数据库配置

基础环境搭建完毕后，需建立基础的数据库配置。在数据库中为每种数据建立数据库。数据库包含以下类型

- 主数据
- 业务数据
- 授权数据
- obd数据库
- 第三方数据库

*注意：若数据库不在同一台服务器上，需在不同的机器中建立数据库。在数据库导入时也需要在不同的服务器上运行脚本。*

## 2.3 项目配置 ##
项目配置文件在`pom.xml`文件中。`pom.xml`文件中的properties标签包含以下配置：

其中`XXX.tomcat.username`和`XXX.tomcat.password`分别为tomcat服务器的管理用户和密码，该信息在2.1安装基础环境时输入。

需配置的服务包括：

- 主业务服务器

	提供系统基础业务逻辑的支撑，包括网页端和移动端的访问接口，以及开放接口等。该服务依赖权限数据库、业务数据库和主数据库的正确配置。

- 网页服务器

	提供网页服务的访问，页面、图片等存放于该服务器中。该服务器需链接redis服务以作session保持。

- obd服务器

	obd服务器主要提供车辆位置、状态等相关查询，该服务依赖obd数据库的正确配置。

- 第三方接口服务器

	提供第三方数据推送，数据查询的功能，依赖于第三方数据库的正确配置

- 推送服务器

	推送服务器目前与业务服务器合并，不单独作配置

-------------------------

	<!-- ================ 基础配置 ============= -->
	<!-- 主业务服务器配置 -->
	<business.server.ip>220.178.67.250:8080</business.server.ip>
	<business.tomcat.username>flasher</business.tomcat.username>
	<business.tomcat.password>flash</business.tomcat.password>

	<!-- 网页服务器配置 -->
	<web.server.ip>220.178.67.250:8080</web.server.ip>
	<web.tomcat.username>flasher</web.tomcat.username>
	<web.tomcat.password>flash</web.tomcat.password>

	<!-- obd服务器配置 -->
	<obd.server.ip>220.178.67.250:8080</obd.server.ip>
	<obd.tomcat.username>flasher</obd.tomcat.username>
	<obd.tomcat.password>flash</obd.tomcat.password>

	<!-- 第三方接口服务器配置 -->
	<thirdParty.server.ip>220.178.67.250:8080</thirdParty.server.ip>
	<thirdParty.tomcat.username>flasher</thirdParty.tomcat.username>
	<thirdParty.tomcat.password>flash</thirdParty.tomcat.password>

	<!-- 推送服务器配置 -->
	<push.server.ip>${business.server.ip}</push.server.ip>
	<push.tomcat.username>${business.tomcat.username}</push.tomcat.username>
	<push.tomcat.password>${business.tomcat.password}</push.tomcat.password>

	<!-- ============数据库配置============== -->
	<!-- 主数据 -->
	<jdbc.main.url>jdbc:mysql://220.178.67.250:3306/iov.vmp.main</jdbc.main.url>
	<jdbc.main.user>root</jdbc.main.user>
	<jdbc.main.pass>flash</jdbc.main.pass>
	<!-- 业务数据 -->
	<jdbc.business.url>jdbc:mysql://220.178.67.250:3306/iov.vmp</jdbc.business.url>
	<jdbc.business.user>root</jdbc.business.user>
	<jdbc.business.pass>flash</jdbc.business.pass>
	<!-- 授权数据 -->
	<jdbc.auth.url>jdbc:mysql://220.178.67.250:3306/iov.auth</jdbc.auth.url>
	<jdbc.auth.user>root</jdbc.auth.user>
	<jdbc.auth.pass>flash</jdbc.auth.pass>
	<!-- obd数据库 -->
	<jdbc.obd.url>jdbc:mysql://220.178.67.250:3306/iov.vmp.obd</jdbc.obd.url>
	<jdbc.obd.user>root</jdbc.obd.user>
	<jdbc.obd.pass>flash</jdbc.obd.pass>
	<!-- 第三方数据库 -->
	<jdbc.thirdParty.url>jdbc:mysql://220.178.67.250:3306/iov.vmp.thirdParty</jdbc.thirdParty.url>
	<jdbc.thirdParty.user>root</jdbc.thirdParty.user>
	<jdbc.thirdParty.pass>flash</jdbc.thirdParty.pass>

将以上配置修改为对应的配置后，执行`prepare.sh`脚本文件，即可测试部署的正确性，若配置没有错误，即可看到如下输出：

## 2.4 基础数据导入

完成以上步骤后，若数据库配置正确，基础的数据表应当已经建立。则可以开始导入基础数据。依次在各数据库服务器上运行相应的`create_XXX_database.sh`脚本，导入相应的基础数据。

## 2.5 正式部署

完成以上工作后，若没有出现错误。则在任何一台服务器上运行部署脚本`deploy.sh`即可。

## 2.6 部署结果测试

部署完成后打开配置的business.server.ip/Monitor即可看到监控服务。若所有服务正常启动，则部署已完成。

# 3 移动端安装部署

## 3.1 信鸽推送服务申请

### 3.1.1 Android应用创建

### 3.1.2 iOS应用创建

## 3.2 ionic环境安装

## 3.3 推送插件安装

## 3.4 编译与发布

### 3.4.1 Android编译环境配置

### 3.4.2 iOS编译环境配置

### 3.4.3 应用发布

# 4 常见问题 ##

	描述常见的问题及处理方式

1. 部署应用时出现测试错误
	
		aaa

2. aaa
2. 