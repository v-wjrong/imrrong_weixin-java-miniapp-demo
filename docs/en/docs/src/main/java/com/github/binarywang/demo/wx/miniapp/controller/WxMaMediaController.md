# Basic Information

|      |      |
|------|------|
| Name | WxMaMediaController |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxMaMediaController.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.controller |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'com.google.common.collect.Lists', 'com.google.common.io.Files', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'org.springframework.web.bind.annotation', 'org.springframework.web.multipart.MultipartFile', 'org.springframework.web.multipart.MultipartHttpServletRequest', 'org.springframework.web.multipart.commons.CommonsMultipartResolver', 'javax.servlet.http.HttpServletRequest', 'java.io.File', 'java.io.IOException', 'java.util.Iterator', 'java.util.List'] |
| Brief Description | WeChat Mini Program Material Controller, providing upload and download functions for temporary materials. Upload requires appid verification and handles multiple files, returning a list of media_ids; download requires appid verification and returns the material file. |

# Description

This is a WeChat Mini Program media file management controller class, which includes functions for uploading and downloading temporary materials. The upload interface receives the appid and HTTP request, processes multi-file uploads after validating configurations, and returns a list of media_ids. The download interface retrieves media files based on the appid and mediaId. Both operations will clear the configuration information stored in ThreadLocal. The upload process involves temporary file creation and exception handling, while the download process directly returns the file object.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaMediaController | class | The WxMaMediaController handles the uploading and downloading of temporary media files for WeChat Mini Programs, including returning a list of media_ids from uploads and retrieving files from downloads, while verifying the validity of the appid. |



## Class WxMaMediaController

|      |      |
|------|------|
| Access Modifier | @RestController;@AllArgsConstructor;@Slf4j;@RequestMapping("/wx/media/{appid}");public |
| Type | class |
| Name | WxMaMediaController |
| Description | The WxMaMediaController handles the uploading and downloading of temporary media files for WeChat Mini Programs, including returning a list of media_ids from uploads and retrieving files from downloads, while verifying the validity of the appid. |


### UML Class Diagram

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

    WxMaMediaController --> WxMaService : Dependency
    WxMaMediaController --> CommonsMultipartResolver : Dependency
    WxMaMediaController --> MultipartHttpServletRequest : Dependency
    WxMaMediaController --> WxMaConfigHolder : Dependency
    WxMaService --> WxMaMediaService : Dependency
    WxMaMediaService --> WxMediaUploadResult : Dependency
    MultipartHttpServletRequest --> MultipartFile : Dependency
```

Class Diagram Description: This diagram illustrates the class structure of a WeChat Mini Program media management controller. WxMaMediaController handles media upload/download via WxMaService, depends on CommonsMultipartResolver for multipart request parsing, uses MultipartHttpServletRequest to retrieve uploaded files, and interacts with WeChat API through WxMaMediaService. WxMaConfigHolder clears thread-local variables, with each interface clearly separating responsibilities across different layers.


### Internal Method Call Graph

```mermaid
graph TD
    A["WxMaMediaController"]
    B["Constructor: Inject wxMaService"]
    C["uploadMedia method"]
    D["Validate appid"]
    E["Create Multipart parser"]
    F["Check request type"]
    G["Parse multipart request"]
    H["Iterate uploaded files"]
    I["Save temporary file"]
    J["Upload to WeChat server"]
    K["Collect media_id"]
    L["Clean ThreadLocal"]
    M["getMedia method"]
    N["Validate appid"]
    O["Download media file"]
    P["Clean ThreadLocal"]

    A --> B
    A --> C
    A --> M
    C --> D["wxMaService.switchover(appid)"]
    D -->|false| D1["Throw exception"]
    D -->|true| E
    E --> F["resolver.isMultipart(request)"]
    F -->|false| L
    F -->|true| G
    G --> H["multiRequest.getFileNames()"]
    H -->|has next| I["Create and save temp file"]
    I --> J["wxMaService.getMediaService().uploadMedia()"]
    J --> K["Add media_id to result set"]
    K --> H
    H -->|no next| L
    M --> N["wxMaService.switchover(appid)"]
    N -->|false| N1["Throw exception"]
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
    alt Invalid appid
        Service-->>Controller: false
        Controller-->>Client: Throw exception
    else Valid appid
        Service-->>Controller: true
        Controller->>Controller: Create Multipart parser
        alt Non-Multipart request
            Controller->>ConfigHolder: remove()
            Controller-->>Client: Return empty list
        else Multipart request
            loop File upload processing
                Controller->>Controller: Get next file
                Controller->>Controller: Save temp file
                Controller->>MediaService: uploadMedia()
                MediaService-->>Controller: Return media_id
                Controller->>Controller: Collect media_id
            end
            Controller->>ConfigHolder: remove()
            Controller-->>Client: Return media_id list
        end
    end

    Client->>Controller: GET /wx/media/{appid}/download/{mediaId}
    Controller->>Service: switchover(appid)
    alt Invalid appid
        Service-->>Controller: false
        Controller-->>Client: Throw exception
    else Valid appid
        Service-->>Controller: true
        Controller->>MediaService: getMedia(mediaId)
        MediaService-->>Controller: Return media file
        Controller->>ConfigHolder: remove()
        Controller-->>Client: Return file
    end
```

This code implements a WeChat Mini Program media file management controller, containing two core functionalities: uploading temporary materials and downloading temporary materials. The upload process validates the appid, parses multipart requests, handles multiple file uploads, and returns a media_id list. The download process similarly validates the appid before retrieving the media file via media_id. Both operations ultimately clean up configuration information stored in ThreadLocal to ensure thread safety. The code includes comprehensive error handling and logging mechanisms.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaService | WxMaService | WeChat Mini Program service instance, private and immutable. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| uploadMedia | List<String> | This is an interface for handling file uploads. After verifying the appid, it receives multiple files, saves them as temporary files, uploads them to the WeChat server, and returns a list of media IDs. |
| getMedia | File | The code is a GET interface designed to download media files based on appid and mediaId. It first checks the validity of the appid, returning an error if invalid. If valid, it retrieves the corresponding file, cleans up the ThreadLocal, and then returns the result. |




