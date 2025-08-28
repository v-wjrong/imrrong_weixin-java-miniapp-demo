# Basic Information

|      |      |
|------|------|
| Name | binarywang |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang |
| Package Name | docs.src.main.java.com.github.binarywang |
| Brief Description | Spring Boot WeChat Mini Program Demo, including startup class, controllers, configurations, error handling, and utility modules. The controllers handle WeChat interactions, configurations manage multiple accounts, and the utility module provides JSON and file processing capabilities. |

# Description

## Overview  
This module is a collection of backend services for WeChat Mini Programs, integrating WeChat ecosystem interactions, configuration management, error handling, and utility functions. Built on the Spring Boot framework, it communicates with WeChat servers via RESTful interfaces and file transfers, operating in a gateway-like mode to handle verification, authorization, and media management. Key data structures include WeChat standard parameters (media_id/appid), configuration properties (WxMaProperties.Config), and MinIO storage strategies. External dependencies include the WeChat JSSDK, Jackson library, MinIO service, and Spring MVC. For example, the media controller handles multi-file uploads, while JsonUtils implements object serialization.

## Core Business Scenarios  
The module covers the entire lifecycle of a Mini Program: server verification (similar to a handshake protocol), user login (OAuth2.0 simplified flow), media hosting (similar to CDN), and error interception. A typical workflow involves receiving parameters → validating configurations → executing business logic → returning data, such as the user controller linking code2session with information decryption. Functional completeness is reflected in multi-account configuration loading, automatic error page redirection (e.g., 404 triggering /error/404), and file upload validation (limited to 50MB). Integration examples include message push processing, temporary media management, and JSON data interaction.


### Package Internal Structure View

```mermaid
graph TD
    binarywang --> demo
    demo --> wx
    wx --> miniapp
    miniapp --> WxMaDemoApplication.java
    miniapp --> controller
    miniapp --> config
    miniapp --> error
    miniapp --> utils
    controller --> WxMaMediaController.java
    controller --> WxMaUserController.java
    controller --> WxPortalController.java
    config --> WxMaProperties.java
    config --> ResourcesConfig.java
    config --> WxMaConfiguration.java
    error --> ErrorController.java
    error --> ErrorPageConfiguration.java
    utils --> JsonUtils.java
    utils --> FileUploadUtils.java
```

This flowchart illustrates the directory structure of a WeChat Mini Program demo project, starting from the root directory binarywang and expanding hierarchically to the miniapp module, which includes submodules such as the main application class, controllers, configurations, error handling, and utility classes. Each submodule contains specific implementation files, such as the three Controller classes under the controller submodule and the three configuration classes under the config submodule, fully presenting the hierarchical relationships within the project.

# File List

| Name   | Type  | Description |
|-------|------|-------------|
| [demo](demo/_module.md) | package | Spring Boot WeChat Mini Program Demo, including startup class, controllers, configurations, error handling, and utility modules. Controllers handle WeChat interactions, configurations manage multiple accounts, and utility modules provide JSON and file processing capabilities. |


