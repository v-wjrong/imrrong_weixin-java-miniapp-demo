[回到首页](../README.md)

## 项目概览
### 项目基本信息
- **名称:** spring-boot-demo-for-wechat-miniapp
- **GroupId (Maven):** com.github.binarywang
- **ArtifactId (Maven):** weixin-java-miniapp-demo
- **Version:** 1.0.0-SNAPSHOT
- **主要编程语言:** Java

## 先决条件
- **JDK 版本:** 1.8 (根据 Maven 的 `<maven.compiler.source>` 和 `<maven.compiler.target>` 属性推断)
- **构建工具版本:** Maven (根据 `pom.xml` 文件识别)
- **网络连接中间件依赖:**
  - Redis (通过 `jedis` 依赖推断，但未明确指定版本)

## 构建指南
### Maven 构建
- 构建命令:
    - 清理构建: `mvn clean`
    - 编译项目: `mvn compile`
    - 打包项目: `mvn package`
    - 安装到本地仓库: `mvn install`
    - 部署项目: `mvn deploy`
- 构建流程: 
    - Maven 的构建流程主要包括以下阶段：
        1. **清理 (clean)**: 删除之前构建生成的文件
        2. **编译 (compile)**: 编译项目源代码
        3. **测试 (test)**: 运行单元测试
        4. **打包 (package)**: 将编译后的代码打包成可分发的格式（如 JAR）
        5. **验证 (verify)**: 对集成测试结果进行检查
        6. **安装 (install)**: 将包安装到本地仓库
        7. **部署 (deploy)**: 将最终的包复制到远程仓库
- 打包目录: 
    - 打包后的 JAR 文件默认生成在 `target/` 目录下，文件名为 `${artifactId}-${version}.jar`
    - 如果使用了 Spring Boot Maven 插件，会生成可执行的 JAR 文件（包含嵌入式 Tomcat）
    - Docker 构建文件位于 `src/main/docker/` 目录，构建后会生成 Docker 镜像

## 依赖管理
### 主要依赖
- **Spring Boot Starter Web**: 用于构建Web应用程序的基础依赖（版本继承自父POM 2.6.3）
- **Spring Boot Starter Thymeleaf**: 集成Thymeleaf模板引擎（版本继承自父POM）
- **weixin-java-miniapp**: 微信小程序Java SDK（显式版本4.7.0通过属性`weixin-java-miniapp.version`管理）
- **commons-fileupload**: 文件上传组件（显式版本1.5）
- **Spring Boot Starter Test**: 测试支持（继承版本，scope为test）
- **Lombok**: 代码简化工具（scope为provided）
- **Jedis**: Redis Java客户端（版本继承自父POM）

### 添加/修改依赖
- **Maven:** 在 `pom.xml` 文件的 `<dependencies>` 标签中添加或修改 `<dependency>` 元素。

### 依赖版本管理
- **Maven (`<dependencyManagement>`):**  
  该POM通过继承`spring-boot-starter-parent`（2.6.3）实现大部分依赖版本管理，部分依赖（如`weixin-java-miniapp`）通过`<properties>`自定义版本。显式声明的依赖（如`commons-fileupload`）会覆盖继承的版本规则。



## 工程结构

```text
weixin-java-miniapp-demo/
├── .editorconfig        # 编辑器配置（标准化配置）
├── .gitignore          # Git忽略规则
├── README.md           # 项目说明文档
├── .travis.yml         # CI/CD配置（Travis CI）
├── pom.xml             # Maven项目配置文件
├── commit.sh           # Git提交脚本（推测）
├── src/
│   ├── main/
│   │   ├── resources/
│   │   │   ├── application.yml.template  # Spring Boot配置模板
│   │   │   ├── tmp.png                   # 临时图片资源（推测）
│   │   │   ├── templates/
│   │   │   │   └── error.html           # 错误页面模板
│   │   │   └── META-INF/
│   │   │       └── additional-spring-configuration-metadata.json  # Spring配置元数据
│   │   ├── java/
│   │   │   └── com/github/binarywang/demo/wx/miniapp/
│   │   │       ├── WxMaDemoApplication.java  # Spring Boot启动类
│   │   │       ├── controller/
│   │   │       │   ├── WxMaMediaController.java  # 微信媒体接口控制器
│   │   │       │   ├── WxMaUserController.java   # 微信用户接口控制器
│   │   │       │   └── WxPortalController.java   # 微信入口控制器
│   │   │       ├── utils/
│   │   │       │   ├── JsonUtils.java       # JSON工具类
│   │   │       │   └── FileUploadUtils.java # 文件上传工具
│   │   │       ├── error/
│   │   │       │   ├── ErrorController.java       # 错误处理控制器
│   │   │       │   └── ErrorPageConfiguration.java  # 错误页面配置
│   │   │       └── config/
│   │   │           ├── WxMaProperties.java     # 微信配置属性类
│   │   │           ├── WxMaConfiguration.java  # 微信配置类
│   │   │           └── ResourcesConfig.java    # 资源路径配置
│   │   └── docker/
│   │       └── Dockerfile  # 容器化部署配置
└── .git/                  # Git版本控制目录（标准结构）
```

命名规约：采用全小写+中划线风格（如wx-miniapp），Java包路径符合域名反转规范（com.github.*）
分层结构：标准Spring Boot分层，包含controller层（接口入口）、config层（配置中心）、utils层（工具类）、error层（统一异常处理）
扩展设计：通过独立docker目录支持容器化部署，resources/templates实现前后端分离的错误页渲染