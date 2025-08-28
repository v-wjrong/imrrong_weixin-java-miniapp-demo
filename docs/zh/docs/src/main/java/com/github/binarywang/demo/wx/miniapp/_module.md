# 基础信息

|      |      |
|------|------|
| 名称 | miniapp |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp |
| 包名 | docs.src.main.java.com.github.binarywang.demo.wx.miniapp |
| 概述说明 | JsonUtils工具类处理JSON转换，配置忽略null值并格式化输出。错误处理模块统一管理404/500错误跳转。微信小程序配置模块整合基础属性和消息处理。控制器模块处理媒体文件、用户会话和微信交互。应用入口类WxMaDemoApplication启动Spring Boot应用。 |

# 说明

## 概述  
该模块是微信小程序后端综合系统，核心职责包括JSON数据处理、错误统一处理、多账号配置管理和微信生态交互（如媒体/会话处理）。采用分层架构设计，类似微服务配置中心与网关模式的结合。接口规范涵盖RESTful端点（如/error/404）、微信标准API（如code2session）和文件传输协议。关键数据结构包含WxMaProperties.Config（凭证存储）、ErrorPageRegistry（错误映射）和微信标准参数（media_id等）。外部依赖Spring Boot框架、微信SDK、JSSDK及消息加解密库。例如JsonUtils实现对象序列化，ErrorController处理错误跳转。

## 主要业务场景  
模块支持小程序全生命周期管理：初始化阶段加载多账号配置（类似配置中心），运行时处理用户授权（OAuth2.0简化流程）、媒体资源托管（类似CDN）和异常拦截（统一错误页）。采用责任链模式处理微信消息（如文本/图片），类似事件总线的分发机制。典型场景包括服务器验证握手、临时素材管理及异步消息响应。例如门户控制器同时处理GET/POST请求，WxMaConfiguration动态路由消息类型。API集成案例覆盖从凭证校验到文件上传的微信标准场景。


### 包内部结构视图

```mermaid
graph TD
    miniapp --> WxMaDemoApplication.java
    miniapp --> utils
    miniapp --> error
    miniapp --> config
    miniapp --> controller
    utils --> JsonUtils.java
    error --> ErrorController.java
    error --> ErrorPageConfiguration.java
    config --> WxMaProperties.java
    config --> ResourcesConfig.java
    config --> WxMaConfiguration.java
    controller --> WxMaMediaController.java
    controller --> WxMaUserController.java
    controller --> WxPortalController.java
```

该流程图展示了微信小程序Demo项目的核心结构，以miniapp为根节点，包含5个主要模块：主应用类、工具类、错误处理类、配置类和控制器类。每个模块下细分具体功能文件，如配置类包含微信小程序属性配置、资源配置等文件，控制器类处理媒体、用户和门户相关请求。结构清晰体现了典型Spring Boot应用的分层设计。

# 文件列表

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [WxMaDemoApplication.java](WxMaDemoApplication.md) | file | 这是一个Spring Boot应用的主类，使用@SpringBootApplication注解标记，通过main方法启动应用。 |
| [controller](controller/_module.md) | package | 微信小程序三个控制器类：媒体控制器处理文件上传下载；用户控制器管理登录、用户信息和手机号；后台控制器处理微信服务器认证和消息推送。均含配置清理功能。 |
| [config](config/_module.md) | package | 微信小程序Java配置类：WxMaProperties绑定小程序配置项；ResourcesConfig处理文件上传和跨域；WxMaConfiguration初始化服务并配置消息处理器。 |
| [error](error/_module.md) | package | ErrorController类处理/error路径下的404和500错误，返回统一错误页面。ErrorPageConfiguration类实现错误页面配置，将404和500状态码映射到对应路径。 |
| [utils](utils/_module.md) | package | JsonUtils类提供静态方法toJson，使用ObjectMapper将对象转为JSON字符串，自动忽略null值并格式化输出。异常时返回null。 |


