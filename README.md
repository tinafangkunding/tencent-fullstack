# tencent-fullstack

简体中文 | [English](./README.en.md)

## 操作场景

该模板可以快速部署一个基于 Vue + Express + PostgreSQL 的全栈 Serverless 应用。主要包含以下组件：
- Serverless RESTful API：通过**云函数**和 **API 网关**构建的 Express 框架实现 RESTful API。
- Serverless 静态网站：前端通过托管 Vue.js 静态页面到 **COS 对象存储**中。
- PostgreSQL Serverless：通过创建 **PostgreSQL DB** 为全栈网站提供数据库服务。
- VPC：通过创建 **VPC** 和 **子网**，提供 SCF 云函数和数据库的网络打通和使用。

## 前提条件
- 已安装 [Node.js](https://nodejs.org/en/)（Node.js 版本需不低于8.6，建议使用 Node.js10.0 及以上版本）


## 操作步骤
### 安装
通过 npm 全局安装 [Serverless Framework](https://github.com/serverless/serverless)：

```shell
$ npm install -g serverless
```

如果之前您已经安装过 Serverless Framework，可以通过下列命令升级到最新版：

```shell
$ npm update -g serverless
```

安装完毕后，通过运行serverless -v命令，查看 Serverless Framework 的版本信息，确保版本信息不低于以下版本：

```shell
$ serverless –v
Framework Core: 1.67.3
Plugin: 3.6.6
SDK: 2.3.0
Components: 2.30.1
```


### 配置
1.新建一个本地文件夹，使用`create --template-url`命令，下载相关 template。
```console
serverless create --template-url https://github.com/serverless-components/tencent-fullstack/tree/master/fullstack-serverless-db
```

2.在项目模板中找到.env.example文件，修改名称为.env，并在其中配置对应的腾讯云 SecretId 和 SecretKey 信息、地域可用区及子网等信息。
```text
# .env
TENCENT_SECRET_ID=xxx  // 您账号的SecretId 
TENCENT_SECRET_KEY=xxx // 您账号的SecretKey

# 地域可用区配置
REGION=ap-beijing //资源部署区，该项目中指云函数与静态页面部署区
ZONE=ap-beijing-3 //资源部署可用区 ，该项目中指DB部署所在的可用区

#VPC 配置，云函数需要通过VPC访问DB，因此VPC与DB必须在同一个可用区
VPC_ID=vpc-xxx //必须和ZONE在同一个区
SUBNET_ID=subnet-xxx //必须和ZONE在同一个区

```

> 说明： 
- 如果没有腾讯云账号，请先 [注册新账号](https://cloud.tencent.com/register)。
- 如果已有腾讯云账号，请保证您的账号已经授权了AdministratorAccess权限。  您可以在 [API 密钥管理](https://console.cloud.tencent.com/cam/capi) 中获取 SecretId 和 SecretKey。
- ZONE目前只支持ap-beijing-3 、ap-guangzhou-2、ap-shanghai-2。
- 您可以在子网中获取VPC_ID和SUBNET_ID，**请务必保证和ZONE在同一个可用区**。

3.通过执行以下命令，安装所需依赖：

```console
$ npm run bootstrap
```

### 部署
1.执行以下命令进行部署.

```console
$ npm run deploy
```

2.执行npm run info查看部署信息，该项目部署的信息分三部分：db、api、frontend（前端网站）。

```console
$ npm run info
fullstack-serverless-db-v2@1.0.0 info /root/tencent-fullstack/fullstack-serverless-db
> npm run info:db && npm run info:api && npm run info:frontend


> fullstack-serverless-db-v2@1.0.0 info:db /root/tencent-fullstack/fullstack-serverless-db
> sls info --target=./db


serverless ⚡ framework

Status:       active
Last Action:  deploy (2 hours ago)
Deployments:  17

region:         ap-guangzhou
zone:           ap-guangzhou-2
vpcConfig: 
  subnetId: subnet-aufwknci
  vpcId:    vpc-fj8y3e7h
dBInstanceName: fullstackDB
private: 
  connectionString: postgresql://tencentdb_95kszm75:9%5Ed%2C)7YX1SvP2nx@172.16.250.17:5432/tencentdb_95kszm75
  host:             172.16.250.17
  port:             5432
  user:             tencentdb_95kszm75
  password:         9^d,)7YX1SvP2nx
  dbname:           tencentdb_95kszm75

fullstackDB › Info successfully loaded


> fullstack-serverless-db-v2@1.0.0 info:api /root/tencent-fullstack/fullstack-serverless-db
> sls info --target=./api


serverless ⚡ framework

Status:       active
Last Action:  deploy (3 hours ago)
Deployments:  9

region: ap-guangzhou
apigw: 
  serviceId:   service-5ejfetgs
  subDomain:   service-5ejfetgs-1259787414.gz.apigw.tencentcs.com
  environment: release
  url:         https://service-5ejfetgs-1259787414.gz.apigw.tencentcs.com/release/
scf: 
  functionName: fullstack-api-v2
  runtime:      Nodejs10.15
  namespace:    default

fullstack-api-v2 › Info successfully loaded


> fullstack-serverless-db-v2@1.0.0 info:frontend /root/tencent-fullstack/fullstack-serverless-db
> sls info --target=./frontend


serverless ⚡ framework

Status:       active
Last Action:  deploy (2 hours ago)
Deployments:  8

website: https://fullstack-serverless-db-1259787414.cos-website.ap-guangzhou.myqcloud.com

fullstack-frontend-v2 › Info successfully loaded
```

3.执行npm run remove，可移除项目。

```console
$ npm run remove
```

