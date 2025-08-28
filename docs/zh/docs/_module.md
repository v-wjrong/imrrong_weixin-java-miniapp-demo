# 基础信息

|      |      |
|------|------|
| 名称 | weixin-java-miniapp-demo |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo |
| 概述说明 | 微信小程序后端系统，处理JSON数据、错误管理、多账号配置和微信交互。采用分层架构，支持RESTful和微信API，依赖Spring Boot和微信SDK。管理小程序生命周期，包括授权、媒体处理和异常拦截。 |

# 模块列表

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [_module.md](src/main/java/com/_module.md) | folder | JsonUtils工具类处理JSON转换，配置忽略null值并格式化输出。错误处理模块统一管理404/500错误跳转。微信小程序配置模块整合基础属性和消息处理。控制器模块处理媒体文件、用户会话和微信交互。应用入口类WxMaDemoApplication启动Spring Boot应用。 |


