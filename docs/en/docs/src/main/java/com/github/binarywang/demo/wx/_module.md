# Basic Information

|      |      |
|------|------|
| Name | wx |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx |
| Package Name | docs.src.main.java.com.github.binarywang.demo.wx |
| Brief Description | The JsonUtils utility class handles JSON conversion, configuring to ignore null values and format the output. The error handling module uniformly manages 404/500 error redirections. The WeChat Mini Program configuration module integrates basic properties and message processing. The controller module manages media files, user sessions, and WeChat interactions. The application entry class, WxMaDemoApplication, launches the Spring Boot application. |

# Description

## Overview  
This module serves as a comprehensive backend system for WeChat Mini Programs, with core responsibilities including JSON data processing, unified error handling, multi-account configuration management, and interactions within the WeChat ecosystem (e.g., media/session handling). It adopts a layered architecture design, combining elements similar to a microservices configuration center and a gateway pattern. Interface specifications encompass RESTful endpoints (e.g., `/error/404`), WeChat standard APIs (e.g., `code2session`), and file transfer protocols. Key data structures include `WxMaProperties.Config` (credential storage), `ErrorPageRegistry` (error mapping), and WeChat standard parameters (e.g., `media_id`). External dependencies include the Spring Boot framework, WeChat SDK, JSSDK, and message encryption/decryption libraries. For example, `JsonUtils` handles object serialization, while `ErrorController` manages error redirection.  

## Core Business Scenarios  
The module supports full lifecycle management of Mini Programs: loading multi-account configurations during initialization (similar to a configuration center), handling user authorization (OAuth2.0 simplified flow), media resource hosting (similar to CDN), and exception interception (unified error pages) during runtime. It employs the chain-of-responsibility pattern to process WeChat messages (e.g., text/images), resembling an event bus distribution mechanism. Typical scenarios include server verification handshakes, temporary media management, and asynchronous message responses. For instance, the portal controller handles both GET/POST requests, while `WxMaConfiguration` dynamically routes message types. API integration examples cover standard WeChat scenarios ranging from credential verification to file uploads.


### Package Internal Structure View

```mermaid
graph TD
    wx --> miniapp
    miniapp --> utils
    miniapp --> error
    miniapp --> config
    miniapp --> controller
    miniapp --> WxMaDemoApplication.java
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

This flowchart illustrates the core directory structure of a WeChat Mini Program Demo project. The root node "wx" contains the main module "miniapp", which is divided into four submodules: utility classes (utils), error handling (error), configuration (config), controllers (controller), along with the main application file. Each submodule includes corresponding functional class files, such as the configuration module containing three configuration classes like WxMaProperties, and the controller module comprising three controllers with distinct functionalities. The structure clearly reflects the layered architecture of a typical Spring Boot application.

# File List

| Name   | Type  | Description |
|-------|------|-------------|
| [miniapp](miniapp/_module.md) | package | The JsonUtils utility class handles JSON conversion, configured to ignore null values and format the output. The error handling module uniformly manages 404/500 error redirections. The WeChat Mini Program configuration module integrates basic properties and message processing. The controller module manages media files, user sessions, and WeChat interactions. The application entry class, WxMaDemoApplication, launches the Spring Boot application. |


