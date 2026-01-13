
# weixin-java-miniapp-demo

## Overview
This is a backend service module for WeChat Mini Programs built on Spring Boot. Its core function is to act as a "configurable API gateway and business logic layer" bridging the WeChat ecosystem and enterprise business applications. It addresses challenges in mini program backend development such as complex configuration and scattered logic when integrating with official WeChat services. By providing unified RESTful API proxies, it encapsulates key interactions including media management, user authentication, and message handling.

The project adopts a monolithic microservices architecture. Its core components include a series of HTTP endpoints with dynamic configuration awareness, a Service layer encapsulating WeChat interaction logic, and utility classes for file processing. The overall flow is visualized as a clear "Request Entry → Dynamic Configuration Loading → WeChat SDK Call → Response Return" pipeline, akin to an intelligent routing hub. It dynamically switches the service context based on the mini program identifier (`appid`) carried by each request.

## What is weixin-java-miniapp-demo?
This project consolidates the core functional modules required for a WeChat Mini Program backend. The main modules include a Configuration Center, Media Service, User Service, and Message Service, which operate in synergy: the Configuration Center provides dynamic WeChat connection properties for all requests; the Media Service handles file uploads to WeChat servers; the User Service manages login and sessions; and the Message Service routes and processes events from the WeChat server.

Its technical implementation is based on the Spring Boot Web framework and the official WeChat Java SDK (`weixin-java-miniapp`), exposing RESTful interfaces via the HTTP protocol. For example, the `POST /media/upload` interface receives files, calls the WeChat SDK for upload, and obtains a `media_id`; the `GET /user/login` interface uses a temporary credential (code) to exchange for the user's `openid` and session key. The entire process follows a unified pattern of "Dynamic Configuration Loading → Calling WeChat API → Processing Returned Data."

The integrated application scenarios form a typical mini program backend solution. For instance, in e-commerce, after a user logs in on the frontend, this backend can decrypt and obtain their identity information for order placement; a content management backend can use it to upload product images to WeChat's temporary media library; the WeChat server can also use its provided verification interface and message router (e.g., `WxMaMessageRouter`) to relay user events to business processors, enabling automated replies or data synchronization.

## Quick Navigation

### 💻 Developers

- [Development Guide](summary/dev_guide.md) - Quickly get started with project development


- [Module Description](docs/_module.md) - Detailed explanation of project modules


### 🏗️ Architect

- [System Architecture](summary/system_architecture.md) - System Architecture


### ↔️ API Documentation

- <a href='http://52.237.88.89/wiki/imrrong/weixin-java-miniapp-demo/ad4e92ea03d50881b46030322f079b9c36ca0f0e/api-viewer.html' target='_blank'>API Documentation</a> - API Documentation

