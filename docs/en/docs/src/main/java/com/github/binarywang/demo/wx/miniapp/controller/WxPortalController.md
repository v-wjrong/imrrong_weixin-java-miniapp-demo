# Basic Information

|      |      |
|------|------|
| Name | WxPortalController |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxPortalController.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.controller |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaMessage', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation', 'java.util.Objects'] |
| Brief Description | WeChat Mini Program Controller, handling GET/POST requests, verifying signatures and routing messages, supporting plaintext and AES-encrypted messages, returning success or error responses. |

# Description

This is a backend controller class for a WeChat Mini Program, containing two main interfaces. The GET interface is used for WeChat server authentication verification, receiving parameters such as signature, timestamp, nonce, and echostr, and returning the echostr after signature validation. The POST interface handles WeChat message push notifications, supporting both plaintext and AES-encrypted formats. It automatically parses JSON or XML formatted message bodies based on configuration and processes messages through a message router. Both interfaces will clean up the configuration information stored in ThreadLocal at the end. The controller also includes logging and exception handling functionalities.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxPortalController | class | WeChat Mini Program Controller, handling authentication and message requests, verifying signatures and routing messages, supporting plaintext and AES encryption, returning success or error messages. |



## Class WxPortalController

|      |      |
|------|------|
| Access Modifier | @RestController;@AllArgsConstructor;@RequestMapping("/wx/portal/{appid}");@Slf4j;public |
| Type | class |
| Name | WxPortalController |
| Description | WeChat Mini Program Controller, handling authentication and message requests, verifying signatures and routing messages, supporting plaintext and AES encryption, returning success or error messages. |


### UML Class Diagram

```mermaid
classDiagram
    class WxPortalController {
        -WxMaService wxMaService
        -WxMaMessageRouter wxMaMessageRouter
        +authGet(String appid, String signature, String timestamp, String nonce, String echostr) String
        +post(String appid, String requestBody, String msgSignature, String encryptType, String signature, String timestamp, String nonce) String
        -route(WxMaMessage message) void
    }

    class WxMaService {
        <<Interface>>
        +switchover(String appid) boolean
        +checkSignature(String timestamp, String nonce, String signature) boolean
        +getWxMaConfig() WxMaConfig
    }

    class WxMaMessageRouter {
        +route(WxMaMessage message) void
    }

    class WxMaMessage {
        <<Interface>>
        +fromJson(String json) WxMaMessage
        +fromXml(String xml) WxMaMessage
        +fromEncryptedJson(String json, WxMaConfig config) WxMaMessage
        +fromEncryptedXml(String xml, WxMaConfig config, String timestamp, String nonce, String msgSignature) WxMaMessage
    }

    class WxMaConfig {
        +getMsgDataFormat() String
    }

    class WxMaConfigHolder {
        +remove() void
    }

    WxPortalController --> WxMaService : Dependency
    WxPortalController --> WxMaMessageRouter : Dependency
    WxMaMessageRouter --> WxMaMessage : Dependency
    WxMaService --> WxMaConfig : Dependency
    WxMaMessage --> WxMaConfig : Dependency
```

Class Diagram Description: This diagram illustrates the core structure of the WeChat Portal Controller (WxPortalController), which interacts with WeChat Service (WxMaService) and Message Router (WxMaMessageRouter) through dependency relationships. The controller contains two main methods for authentication (authGet) and request processing (post), which verify signatures, parse message formats (JSON/XML), and route messages when handling requests from the WeChat server. The message processing involves encryption type determination and thread-local configuration cleanup, demonstrating a complete WeChat message handling workflow.


### Internal Method Call Graph

```mermaid
graph TD
    A["WxPortalController Class"]
    B["GET Authentication Endpoint: authGet"]
    C["POST Message Handler: post"]
    D["Internal Routing Method: route"]
    E["Dependency: WxMaService"]
    F["Dependency: WxMaMessageRouter"]

    A --> E
    A --> F
    A --> B
    A --> C
    C --> D

    B --> B1["Parameter Validation: signature/timestamp/nonce/echostr"]
    B1 -->|Invalid| B2["Throw IllegalArgumentException"]
    B1 -->|Valid| B3["Switch appid config: wxMaService.switchover"]
    B3 -->|Failed| B4["Throw IllegalArgumentException"]
    B3 -->|Success| B5["Verify Signature: wxMaService.checkSignature"]
    B5 -->|Pass| B6["Return echostr"]
    B5 -->|Fail| B7["Return 'Invalid Request'"]
    B6 & B7 & B4 & B2 --> B8["Clean ThreadLocal: WxMaConfigHolder.remove"]

    C --> C1["Parameter Validation: appid config"]
    C1 -->|Failed| C2["Throw IllegalArgumentException"]
    C1 -->|Success| C3["Determine Message Format: isJson"]
    C3 --> C4["Plaintext Message Branch"]
    C3 --> C5["AES Encrypted Message Branch"]
    C4 --> C6["Parse Message: fromJson/fromXml"]
    C5 --> C7["Decrypt Message: fromEncryptedJson/fromEncryptedXml"]
    C6 & C7 --> C8["Route Message: route method"]
    C8 --> C9["Return 'success'"]
    C9 & C2 --> C10["Clean ThreadLocal"]
    C3 -->|Unknown Encryption| C11["Throw RuntimeException"]
    C11 --> C10
```

This flowchart illustrates the core logic of the WeChat portal controller, featuring two primary endpoints: GET authentication and POST message handling. The GET endpoint validates WeChat server signatures through a three-layer verification process (parameter null check, appid configuration check, and signature verification) before returning responses. The POST endpoint branches based on encryption types, supporting both plaintext and AES-encrypted message formats, with final message distribution handled by a message router. Both endpoints incorporate ThreadLocal cleanup mechanisms to ensure thread safety.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaMessageRouter | WxMaMessageRouter | WeChat Mini Program Message Routing Object |
| wxMaService | WxMaService | WeChat Mini Program service instance, private and immutable. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| route | void | Route WeChat Mini Program messages and log error messages in case of exceptions. |
| authGet | String | This is a GET interface for handling WeChat server authentication requests, which verifies the signature parameters and returns the echostr or an error message. |
| post | String | The POST interface for handling WeChat requests supports plaintext and AES-encrypted messages. After verifying the appid, it parses and routes messages based on their format (JSON/XML), then cleans up ThreadLocal before returning success or error. |




