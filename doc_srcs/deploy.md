# 部署指南

## 前言

Dmicros设计遵循“适度解藕”的原则：前端界面、后端服务和数据库服务可分离部署，再通过网络通讯联系起来为不同类型的用户提供服务。因此，服务运营方可根据IT基建的易得性选择更为适宜的部署方式。

容器技术使得服务系统容易被分解成小的容器组件分别进行维护，并提供组件性能的伸缩，非常适合部署Dmicros相关应用。以下就将以利用Docker容器技术进行部署为主线，说明Dmicros服务的具体部署方案。

## 基本设计架构

Dmicros服务大致由纯前端APP、API服务和数据库服务等部分构成。其中**https加密能力需要各应用的托管平台提供**。

::uml:: format="svg"

    rectangle "应用层" {
        node "管理应用" as ManagerApp
        node "计算应用"
        node "用户PCWeb应用"
        node "用户Mobile应用"
        node "用户MobileWeb应用"
        interface "web:HTTPS"
    }

    rectangle "数据层" {
        database MongoDB
    }

    rectangle "API层" {
        node "Dmicros API"
        interface "docker:HTTPS"
    }

   rectangle 用户 {
        actor "开发者" as Developer
        actor "终端用户" as User
    }

    [Dmicros API] -- [docker:HTTPS]
    [Dmicros API] -- MongoDB
    [web:HTTPS] -- [用户PCWeb应用]
    [web:HTTPS] -- [用户MobileWeb应用]
    [docker:HTTPS] -(0)- 应用层

    Developer --> [docker:HTTPS]
    User --> [web:HTTPS]
    User --> [用户Mobile应用]

::end-uml::

目前各部分开发进度各有不同,**以下将说明的部署方案只涵盖到API和用户应用**：

  * API层：初步可用，已通过开发测试
  * 用户MobileWeb应用：原型+Demo
  * 管理UI：仅原型
  * 其他应用：尚未开发

## 基于Docker技术的部署方案

服务应用部署主要需要以下三部分：

  * 数据库服务：MongoDB服务
  * API：Docker封装服务，通过MONGO URI链接数据库服务
  * 前端UI：html+javascript静态网页，通过API地址链接API

::uml:: format="svg"

    database MongoDB {
        storage "db:...a"
        storage "db:dmicros"
        storage "db:...x"
    }

    cloud "Docker托管" {
        node ApiImage [
            Dmicros API
            ====
            Docker镜像
        ]
        interface "docker:HTTPS"
        ApiImage -- [docker:HTTPS]
    }

    [db:dmicros] -- ApiImage : 镜像环境变量MONGO_URI

    cloud "静态页面托管" {
        node WebApp [
            用户应用
            ====
            纯前端应用
        ]
        interface "web:HTTPS"
        WebApp -- [web:HTTPS]

    }

    [docker:HTTPS] -- WebApp : API服务根地址

    actor "开发者" as Developer
    actor "终端用户" as User

    Developer --> [docker:HTTPS]
    User --> [web:HTTPS]

::end-uml::

### 基础设施需求：

数据库：

  * MongoDB数据服务中的一个独立数据库（即`dbname`）
  * 对以上该特定数据库的可读写权限、建立索引权限
  * 也可通过Docker封装，但Dmicros并不需要使用独立的MongoDB服务

API托管：

  * Docker服务托管
  * Docker镜像托管
  * https反向代理

前端应用托管：

  * 静态页面托管
  * https反向代理
  * 可通过Docker封装，但建议使用静态托管资源

其他有助于提高效率的需求：

  * 同时部署开发测试版和生产版
  * 域名绑定和SSL证书自动管理
  * Docker持续集成
  * 静态页面持续集成

### 推荐的开发和部署流程

运营方可以通过Docker服务编排刷新部署和合并App源代码库来接受新提交的功能更新：

::uml::

    actor "开发者" as Developer
    boundary "API镜像" as ApiImage
    boundary "应用开发代码库" as AppRepo
    entity "测试服务器" as DevServer
    entity "生产服务器" as ProductionServer
    boundary "应用生产代码库" as AppPRepo
    boundary "API服务编排" as ApiConfig
    actor  "运营方" as Maintainer

    Developer -> ApiImage : 推送更新
    Developer -> AppRepo : 推送更新
    ApiImage --> DevServer : 持续集成
    AppRepo --> DevServer : 持续集成
    Developer -> Maintainer : 提请更新
    Maintainer -> AppPRepo : 合并推送请求
    Maintainer -> ApiConfig : 刷新部署
    ApiConfig --> ProductionServer : 刷新服务
    AppPRepo --> ProductionServer : 持续集成
    ApiConfig --> ApiImage : 应用镜像
    AppRepo --> AppPRepo : 请求推送更新

::end-uml::

## 时速云+coding.net Demo版部署案例

已利用时速云和coding.net平台服务部署了Demo版，具体部署可供参考。

::uml:: format="svg"

    cloud "时速云TenxCloud容器服务" {
        node MongoDB {
            storage "db:dmicros"
        }
        node ApiImage [
            Dmicros API
            ====
            Docker镜像
        ]
        interface "docker:HTTPS"
        ApiImage -- [docker:HTTPS]
    }

    [db:dmicros] -- ApiImage : 镜像环境变量MONGO_URI

    cloud "Coding.net源代码和Pages托管服务" {
        node WebApp [
            用户应用
            ====
            纯前端应用
        ]
        interface "web:HTTPS"
        WebApp -- [web:HTTPS]

    }
    [docker:HTTPS] -- WebApp : API服务根地址

::end-uml::

时速云提供的相关能力：

  * Docker镜像仓库托管
  * 镜像持续构建
  * HTTPS转发

coding.net提供的相关能力：

  * 源代码托管
  * 动静态web服务托管
