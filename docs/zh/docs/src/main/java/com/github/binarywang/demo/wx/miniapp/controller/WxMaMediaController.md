# 基础信息

|      |      |
|------|------|
| 名称 | WxMaMediaController |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxMaMediaController.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.controller |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'com.google.common.collect.Lists', 'com.google.common.io.Files', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'org.springframework.web.bind.annotation', 'org.springframework.web.multipart.MultipartFile', 'org.springframework.web.multipart.MultipartHttpServletRequest', 'org.springframework.web.multipart.commons.CommonsMultipartResolver', 'javax.servlet.http.HttpServletRequest', 'java.io.File', 'java.io.IOException', 'java.util.Iterator', 'java.util.List'] |
| 概述说明 | 微信小程序素材控制器，提供上传和下载临时素材功能。上传需验证appid并处理多文件，返回media_id列表；下载需验证appid并返回素材文件。 |

# 说明

这是一个微信小程序媒体文件管理控制器类，包含上传和下载临时素材功能。上传接口接收appid和HTTP请求，验证配置后处理多文件上传，返回media_id列表。下载接口根据appid和mediaId获取媒体文件。两个操作均会清理ThreadLocal存储的配置信息。上传过程涉及临时文件创建和异常处理，下载过程直接返回文件对象。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxMaMediaController | class | WxMaMediaController处理微信小程序临时素材上传下载，包含上传返回media_id列表和下载返回文件功能，需校验appid有效性。 |



## 类 WxMaMediaController

|      |      |
|------|------|
| 访问范围 | @RestController;@AllArgsConstructor;@Slf4j;@RequestMapping("/wx/media/{appid}");public |
| 类型 | class |
| 名称 | WxMaMediaController |
| 说明 | WxMaMediaController处理微信小程序临时素材上传下载，包含上传返回media_id列表和下载返回文件功能，需校验appid有效性。 |


### UML类图

```mermaid
classDiagram
    class WxMaMediaController {
        -WxMaService wxMaService
        +uploadMedia(String appid, HttpServletRequest request) List~String~
        +getMedia(String appid, String mediaId) File
    }

    class WxMaService {
        <<Interface>>
        +switchover(String appid) boolean
        +getMediaService() WxMaMediaService
    }

    class WxMaMediaService {
        <<Interface>>
        +uploadMedia(String type, File file) WxMediaUploadResult
        +getMedia(String mediaId) File
    }

    class WxMediaUploadResult {
        -String mediaId
        +getMediaId() String
    }

    class CommonsMultipartResolver {
        +isMultipart(HttpServletRequest request) boolean
    }

    class MultipartHttpServletRequest {
        <<Interface>>
        +getFileNames() Iterator~String~
        +getFile(String name) MultipartFile
    }

    class MultipartFile {
        <<Interface>>
        +getOriginalFilename() String
        +transferTo(File dest) void
    }

    class WxMaConfigHolder {
        <<Utility>>
        +remove() void
    }

    WxMaMediaController --> WxMaService : 依赖
    WxMaMediaController --> CommonsMultipartResolver : 依赖
    WxMaMediaController --> MultipartHttpServletRequest : 依赖
    WxMaMediaController --> WxMaConfigHolder : 依赖
    WxMaService --> WxMaMediaService : 依赖
    WxMaMediaService --> WxMediaUploadResult : 依赖
    MultipartHttpServletRequest --> MultipartFile : 依赖
```

类图描述：该图展示了一个微信小程序素材管理控制器的类结构。WxMaMediaController通过WxMaService处理素材上传下载，依赖CommonsMultipartResolver解析多部分请求，使用MultipartHttpServletRequest获取上传文件，并通过WxMaMediaService与微信API交互。WxMaConfigHolder用于清理线程本地变量，各接口清晰地分离了不同层级的职责。


### 内部方法调用关系图

```mermaid
graph TD
    A["WxMaMediaController"]
    B["构造方法: 注入wxMaService"]
    C["uploadMedia方法"]
    D["检查appid有效性"]
    E["创建Multipart解析器"]
    F["检查请求类型"]
    G["解析多部分请求"]
    H["遍历上传文件"]
    I["保存临时文件"]
    J["上传到微信服务器"]
    K["收集media_id"]
    L["清理ThreadLocal"]
    M["getMedia方法"]
    N["检查appid有效性"]
    O["下载媒体文件"]
    P["清理ThreadLocal"]

    A --> B
    A --> C
    A --> M
    C --> D["wxMaService.switchover(appid)"]
    D -->|false| D1["抛出异常"]
    D -->|true| E
    E --> F["resolver.isMultipart(request)"]
    F -->|false| L
    F -->|true| G
    G --> H["multiRequest.getFileNames()"]
    H -->|有下一个| I["创建临时文件并保存"]
    I --> J["wxMaService.getMediaService().uploadMedia()"]
    J --> K["添加media_id到结果集"]
    K --> H
    H -->|无下一个| L
    M --> N["wxMaService.switchover(appid)"]
    N -->|false| N1["抛出异常"]
    N -->|true| O["wxMaService.getMediaService().getMedia(mediaId)"]
    O --> P
```

```mermaid
sequenceDiagram
    participant Client
    participant Controller as WxMaMediaController
    participant Service as WxMaService
    participant MediaService as MediaService
    participant ConfigHolder as WxMaConfigHolder

    Client->>Controller: POST /wx/media/{appid}/upload
    Controller->>Service: switchover(appid)
    alt 无效appid
        Service-->>Controller: false
        Controller-->>Client: 抛出异常
    else 有效appid
        Service-->>Controller: true
        Controller->>Controller: 创建Multipart解析器
        alt 非Multipart请求
            Controller->>ConfigHolder: remove()
            Controller-->>Client: 返回空列表
        else Multipart请求
            loop 文件上传处理
                Controller->>Controller: 获取下一个文件
                Controller->>Controller: 保存临时文件
                Controller->>MediaService: uploadMedia()
                MediaService-->>Controller: 返回media_id
                Controller->>Controller: 收集media_id
            end
            Controller->>ConfigHolder: remove()
            Controller-->>Client: 返回media_id列表
        end
    end

    Client->>Controller: GET /wx/media/{appid}/download/{mediaId}
    Controller->>Service: switchover(appid)
    alt 无效appid
        Service-->>Controller: false
        Controller-->>Client: 抛出异常
    else 有效appid
        Service-->>Controller: true
        Controller->>MediaService: getMedia(mediaId)
        MediaService-->>Controller: 返回媒体文件
        Controller->>ConfigHolder: remove()
        Controller-->>Client: 返回文件
    end
```

这段代码实现了一个微信小程序媒体文件管理控制器，包含上传临时素材和下载临时素材两个核心功能。上传流程会验证appid、解析多部分请求、处理多个文件上传并返回media_id列表；下载流程同样验证appid后通过media_id获取媒体文件。两个操作最后都会清理ThreadLocal存储的配置信息，确保线程安全。代码中包含了完善的错误处理和日志记录机制。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaService | WxMaService | 微信小程序服务实例，私有不可变。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| uploadMedia | List<String> | 这是一个处理文件上传的接口，验证appid后接收多文件，保存为临时文件并上传至微信服务器，返回媒体ID列表。 |
| getMedia | File | 该代码是一个GET接口，用于根据appid和mediaId下载媒体文件。首先检查appid有效性，无效则报错；有效则获取对应文件并清理ThreadLocal后返回。 |




