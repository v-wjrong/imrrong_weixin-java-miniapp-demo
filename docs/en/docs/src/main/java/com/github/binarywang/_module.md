# Basic Information

|      |      |
|------|------|
| Name | binarywang |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang |
| Package Name | docs.src.main.java.com.github.binarywang |
| Brief Description | This is a demonstration of a backend project for a WeChat Mini Program based on Spring Boot. The project includes the main startup class, controllers, utility classes, error handling, and configuration modules. The controllers handle WeChat API requests, such as user login and file uploads. The utility classes provide JSON processing and file storage functionality. The error handling module customizes HTTP error pages. The configuration module centrally manages Mini Program settings and message routing. The overall architecture is clear and covers the core backend functionalities of the Mini Program. |

# Description

## Overview
This is a backend service module for a WeChat Mini Program based on Spring Boot. Its core responsibility is to provide a unified API proxy for interacting with official WeChat services and encapsulate key business logic such as media file management, user identity authentication, and message/event handling. It acts as a bridge between the business application and the WeChat ecosystem.

The module adheres to RESTful interface specifications, providing a series of HTTP endpoints. In terms of design patterns, it dynamically loads the corresponding configuration based on `appid` before processing each request. Its key data structures include configuration objects encapsulating Mini Program connection properties, `media_id` returned by WeChat, session information containing `openid`, and encrypted data packets, all of which flow between controllers. External dependencies mainly include the Spring Boot Web framework, the WeChat Java SDK (`weixin-java-miniapp`), and libraries such as Jackson and MinIO.

Specific implementation examples are plentiful. For instance: the `POST /media/upload` interface receives and uploads files to the WeChat server; the `GET /user/login` endpoint uses a code to exchange for a user session; the utility class `JsonUtils` configures an `ObjectMapper` for JSON serialization; `FileUploadUtils` validates extensions and renames files before upload.

## Main Business Scenarios
The module's business covers three major processes: file resource management, user identity data management, and server message/event handling, forming a complete view from receiving a request to calling WeChat services and returning a response. Its interaction pattern is unified as "dynamically load configuration → call WeChat API → process returned data → clean thread context," similar to a configuration-aware routing layer.

Functional completeness is reflected in providing core backend interfaces required for Mini Program development, including temporary media upload, decryption of user login information, server configuration verification, and message dispatching. A typical application pattern is: after the Mini Program frontend completes user login, user information can be obtained; the management backend can upload media; and the WeChat server communicates events with the backend through verification interfaces.

API types are mainly HTTP interfaces and static methods of utility classes. Integration examples include: `WxMaMessageRouter` automatically routes messages to handlers based on message type; the error handling module maps 404/500 errors to customized pages; the configuration module centrally manages multiple Mini Program properties and initializes core service Beans.


### Package Internal Structure View

```mermaid
graph TD
    binarywang --> demo
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

This flowchart depicts the core Java package structure of the WeChat Mini Program demo project. The root node is `binarywang`, which contains the `demo` package, further extending to the `wx` and `miniapp` core packages. The `miniapp` package serves as the core, directly containing the main application class `WxMaDemoApplication.java` and the four main functional module packages: `controller`, `utils`, `error`, and `config`. Each functional module package further contains its specific implementation classes, clearly illustrating the project's hierarchical organization and module division.

# File List

| Name   | Type  | Description |
|-------|------|-------------|
| [demo](demo/_module.md) | package | This is a backend demonstration project for a WeChat Mini Program based on Spring Boot. The project includes a main startup class, controllers, utility classes, error handling, and configuration modules. The controllers handle WeChat API requests, such as user login and file uploads. The utility classes provide JSON processing and file storage functionalities. The error handling module customizes HTTP error pages. The configuration module centrally manages Mini Program settings and message routing. The overall architecture is clear and covers the core functionalities of a Mini Program backend. |


