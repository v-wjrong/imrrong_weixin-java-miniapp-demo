# Basic Information

|      |      |
|------|------|
| Name | weixin-java-miniapp-demo |
| Language | .java |
| Code Path | weixin-java-miniapp-demo |
| Brief Description | WeChat Mini Program backend system, handling JSON data, error management, multi-account configuration, and WeChat interactions. Adopts a layered architecture, supports RESTful and WeChat APIs, and relies on Spring Boot and WeChat SDK. Manages the Mini Program lifecycle, including authorization, media processing, and exception interception. |

# Module List

| Name   | Type  | Description |
|-------|------|-------------|
| [_module.md](src/main/java/com/_module.md) | folder | The JsonUtils utility class handles JSON conversion, configured to ignore null values and format the output. The error handling module uniformly manages 404/500 error redirections. The WeChat Mini Program configuration module integrates basic properties and message processing. The controller module handles media files, user sessions, and WeChat interactions. The application entry class, WxMaDemoApplication, launches the Spring Boot application. |


