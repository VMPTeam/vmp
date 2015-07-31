#安装部署手册

# 1  概述 #

## 1.1  硬件环境 ##

本项目至少需要一台计算机来满足项目的部署需求。本说明按以下硬件配置为例进行安装部署说明：

![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/hardware.png)

应用服务部署在应用层ECS中。数据库采用阿里云RDS存储方案。最后由多台ECS提供kafka队列、Redis缓存服务支持。

## 1.2  软件环境 ##

本项目主要使用以下软件，在使用前请确保可以安装以下软件

* tomcat7
* jdk1.6
* maven
* MySQL
* 卡夫卡队列
* Redis缓存

## 1.3 如何获取源代码

源代码使用SVN管理，访问地址为：`http://scm.wantong-tech.net:1080/iov.vmp/`。我们建议从tags文件夹中获取最新的稳定发布版本进行实际发布。

![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/tags.png)

# 2 安装步骤

为尽量减少安装难度，源代码的main目录下编写了bash脚本以方便用户快速部署。所有的脚本请在main目录下执行。

## 2.1 基础环境搭建

本项目依赖`tomcat7-admin`环境和`maven`项目管理等软件进行安装部署。可运行main目录下的`base_environment_install.sh`文件进行快速的基础环境搭建。
基础环境搭建需在所有分布式服务器上运行，以满足项目基础配置需求。

## 2.2 数据库配置

基础环境搭建完毕后，需建立基础的数据库配置。在数据库中为每种数据建立数据库。数据库包含以下类型

- 主数据 - main
- 业务数据 - business
- 授权数据 - auth
- obd数据库 - obd
- 第三方数据库 - thirdParty

依次运行`create_XXX_database.sh`系列脚本（不分先后），输入远端数据库的ip，数据库root密码，数据库名称后，脚本即可在远程客户机上创建对应的数据库。

*注：若输入的数据库地址不为本地，请注意远端MySQL数据库是否开启了远程访问权限（默认关闭）。*

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

	推送服务器目前与业务服务器合并，不单独作配置。推送的相关信息，如id,token等请参考3.1信鸽服务申请章节。

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

	<!-- ================ 推送服务配置 =================== -->
	<!-- android -->
	<android.push.id>2100133203</android.push.id>
	<android.push.token>b9259d845acf3dbdf41db11e147b55e8</android.push.token>
	<!-- ios -->
	<ios.push.id>2200133204</ios.push.id>
	<ios.push.token>4d362fc85b9c7dda7f189b5b9bd024c7</ios.push.token>

将以上配置修改为对应的配置后，执行`prepare.sh`脚本文件，即可测试部署的正确性，若配置没有错误，即可看到如下输出：

![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/succeed.png)

*注：准备过程不会部署实际代码，但是会操作远程机器的数据库以便测试整个环境的可用性。*

*准备过程中将动态下载项目所需Java库，请保证网络通畅*

## 2.4 正式部署

以上操作若没有出现错误，则可以开始正式部署系统。系统部署分为批量部署和单独部署方式。批量部署是简单的快速部署方式，可快速建立可用的服务架构。但是批量部署不能建立分布式的解决方案，对于分布式的架构，需手动部署相关安装包。

### 2.4.1 批量部署

完成以上工作后，若没有出现错误。则在任何一台服务器上运行部署脚本`deploy.sh`即可。该脚本将把项目依次部署到设置的服务器中，并连接相应的数据库和Redis。

以本文开头的架构为例，只需在应用层两台ECS上分别配置并运行`prepare.sh`和`deploy.sh`即可正常使用。

### 2.4.2 手动部署

手动部署建立在批量部署成功的基础上，对相应的模块进行配置并部署为并行的分布式解决方案。

根据架构方案的不同，手动部署需将准备好的war包部署到对应服务器上。部署流程如下：

1. 首先配置不同的参数满足对应服务器的需求。具体配置方法见2.3。其中，Web服务器和business服务器是常见的需要做分布式的服务类型，修改他们新的服务器地址即可配置新的解决方案。
2. 运行`prepare.sh`文件验证配置的正确性并编译代码
3. 从对应项目的target文件夹下（如`WebApplication/target`）找到war包，并上传部署到对应主机的tomcat服务器上。

## 2.5 支持服务部署

本项目依赖两个后台服务提供OBD服务支持，包括一个OBD信息处理服务和一个数据库维护服务。在完成正式环境部署后，还不能获取OBD的实时信息，此时需部署该后台服务。

除基础环境外，服务架构中需至少有一台机器运行kafka队列服务，请使用`install_kafka.sh`安装kafka运行环境。

若手动安装，请将kafka server接口调整为2188。

在需要部署后台服务的机器上运行`install_backend.sh`文件来部署两个后台服务。该文件依赖obd数据库、卡夫卡服务和Redis服务的正确配置。以本文中的架构为例，该服务可部署在计算ECS中。

## 2.6 部署结果测试

部署完成后打开配置的服务器的`/Monitor`服务即可看到监控。若所有服务正常启动，则部署已完成。

![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/monitor.png)

# 3 移动端安装部署

## 3.1 信鸽推送服务申请

1. 在腾讯[http://xg.qq.com](http://xg.qq.com "xg.qq.com")网站申请腾讯开发者账号（qq账号可直接登录）。
	
	![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/xg_login.png)

1. 登录后即可创建新的推送应用。其中Android应用需正确填写包名。

	![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/xg_register.png)

1. 应用成果创建后，iOS需上传开发者证书和实际环境证书。证书制作与上传请参考官方手册：
[iOS证书制作指南](http://developer.xg.qq.com/index.php/IOS_%E8%AF%81%E4%B9%A6%E8%AE%BE%E7%BD%AE%E6%8C%87%E5%8D%97 "iOS证书制作指南")

	![image](https://github.com/VMPTeam/vmp/raw/master/docs/07InstallationDeployment/images/xg_cert.png)

1. 记下所申请的应用access_id，access_key，access_token，并正确配置到应用中。

## 3.2 ionic环境安装

## 3.3 推送插件安装

## 3.4 编译与发布

### 3.4.1 Android编译环境配置

### 3.4.2 iOS编译环境配置

### 3.4.3 应用发布

# 4 常见问题 ##

	描述常见的问题及处理方式

1. 部署应用时出现测试错误
	
		请检查配置文件是否严格按原格式设置
