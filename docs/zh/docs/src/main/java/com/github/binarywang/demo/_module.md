# 基础信息

|      |      |
|------|------|
| 名称 | demo |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo |
| 包名 | docs.src.main.java.com.github.binarywang.demo |
| 概述说明 | 这是一个基于Spring Boot的微信小程序后端演示项目。项目包含主启动类、控制器、工具类、错误处理和配置模块。控制器处理微信API请求，如用户登录和文件上传。工具类提供JSON处理和文件存储功能。错误处理模块定制HTTP错误页面。配置模块集中管理小程序设置和消息路由。整体架构清晰，覆盖小程序后端核心功能。 |

# 说明

## 概述
这是一个基于Spring Boot的微信小程序后端服务模块，核心职责是提供与微信官方服务交互的统一API代理，并封装关键业务逻辑如媒体文件管理、用户身份认证与消息事件处理。它充当业务应用与微信生态之间的桥梁。

模块遵循RESTful接口规范，提供一系列HTTP端点，设计模式上会在每个请求处理前根据`appid`动态加载对应配置。其关键数据结构包括封装小程序连接属性的配置对象、微信返回的`media_id`、包含`openid`的会话信息以及加密数据包，这些数据在控制器间流转。外部依赖主要包括Spring Boot Web框架、微信Java SDK（`weixin-java-miniapp`）以及Jackson和MinIO等库。

具体实现案例丰富，例如：通过`POST /media/upload`接口接收并上传文件至微信服务器；通过`GET /user/login`使用code换取用户会话；工具类`JsonUtils`配置`ObjectMapper`进行JSON序列化；`FileUploadUtils`上传文件前校验扩展名并重命名。

## 主要业务场景
模块业务覆盖文件资源管理、用户身份数据管理和服务器消息事件处理三大流程，形成了从接收请求到调用微信服务并返回响应的完整视图。其交互模式统一为“动态加载配置→调用微信API→处理返回数据→清理线程上下文”，类似一个配置感知的路由层。

功能完整性体现在提供了小程序开发所需的核心后端接口，包括临时素材上传、用户登录信息解密、服务器配置验证与消息分发。典型应用模式是：小程序前端完成用户登录后，可获取用户信息；管理后台可上传素材；微信服务器通过验证接口与后端进行事件通信。

API类型主要为HTTP接口和工具类静态方法，集成案例如：`WxMaMessageRouter`根据消息类型自动路由至处理器；错误处理模块将404/500错误映射到定制页面；配置模块集中管理多小程序属性并初始化核心服务Bean。


### 包内部结构视图

```mermaid
graph TD
    demo --> wx
    wx --> miniapp
    miniapp --> WxMaDemoApplication.java
    miniapp --> controller
    miniapp --> utils
    miniapp --> error
    miniapp --> config
    controller --> WxMaMediaController.java
    controller --> WxMaUserController.java
    controller --> WxPortalController.java
    utils --> JsonUtils.java
    utils --> FileUploadUtils.java
    error --> ErrorPageConfiguration.java
    error --> ErrorController.java
    config --> WxMaProperties.java
    config --> WxMaConfiguration.java
    config --> ResourcesConfig.java
```

该流程图展示了 `weixin-java-miniapp-demo` 项目 `src/main/java` 目录下一个典型的分层结构。根节点 `demo` 包含 `wx` 子包，其下是核心包 `miniapp`。`miniapp` 包包含一个主应用类 `WxMaDemoApplication.java` 以及四个子包：`controller`（存放各类控制器）、`utils`（工具类）、`error`（错误处理类）和 `config`（配置类）。每个子包下均包含了相应的具体功能实现文件，清晰地呈现了代码的组织方式与模块间的层级关系。

# 文件列表

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [wx](wx/_module.md) | package | 这是一个基于Spring Boot的微信小程序后端演示项目。项目包含主启动类、控制器、工具类、错误处理和配置模块。控制器处理微信API请求，如用户登录和文件上传。工具类提供JSON处理和文件存储功能。错误处理模块定制HTTP错误页面。配置模块集中管理小程序设置和消息路由。整体架构清晰，覆盖小程序后端核心功能。 |


