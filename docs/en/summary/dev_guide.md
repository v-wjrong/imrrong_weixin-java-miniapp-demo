[Go to Homepage](../README.md)

## Project Overview  
### Basic Project Information  
- **Name:** spring-boot-demo-for-wechat-miniapp  
- **GroupId (Maven):** com.github.binarywang  
- **ArtifactId (Maven):** weixin-java-miniapp-demo  
- **Version:** 1.0.0-SNAPSHOT  
- **Primary Programming Language:** Java  

## Prerequisites  
- **JDK Version:** 1.8 (inferred from Maven's `<maven.compiler.source>` and `<maven.compiler.target>` properties)  
- **Build Tool Version:** Maven (identified as `pom.xml` based on file structure and content, but no explicit Maven version is specified; it is recommended to use the latest version compatible with Spring Boot 2.6.3)  
- **Network Middleware Dependencies:**  
  - Redis (inferred from the `jedis` dependency, but no explicit version is specified)  

## Build Guide  
### Maven Build  
- Build Commands:  
    - Clean build: `mvn clean`  
    - Compile project: `mvn compile`  
    - Package project: `mvn package`  
    - Install to local repository: `mvn install`  
    - Deploy project: `mvn deploy`  
- Build Process:  
    - Maven's build process mainly includes the following phases: clean, compile, test, package, verify, install, and deploy. The project uses the Spring Boot Maven plugin, so a runnable JAR file is generated after packaging.  
- Packaging Directory:  
    - The packaged file is located in the `target/` directory by default, with the filename `${project.artifactId}-${project.version}.jar` (e.g., `weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`).  
    - If the Docker plugin is used, Docker-related configuration files and resources are located in the `src/main/docker/` directory.  

## Dependency Management  
### Key Dependencies  
- **Spring Boot Web:** `org.springframework.boot:spring-boot-starter-web` - Provides Spring Boot Web functionality (version inherited from parent POM 2.6.3)  
- **Thymeleaf:** `org.springframework.boot:spring-boot-starter-thymeleaf` - Template engine support (version inherited from parent POM)  
- **WeChat Mini Program SDK:** `com.github.binarywang:weixin-java-miniapp:4.7.0` - WeChat Mini Program Java SDK  
- **File Upload:** `commons-fileupload:commons-fileupload:1.5` - Apache Commons file upload component  
- **Testing:** `org.springframework.boot:spring-boot-starter-test` - Spring Boot testing support (scope=test)  
- **Lombok:** `org.projectlombok:lombok` - Annotation-driven code generation tool (scope=provided)  
- **Redis Client:** `redis.clients:jedis` - Redis Java client (version inherited from parent POM)  

### Adding/Modifying Dependencies  
- **Maven:** Add or modify `<dependency>` elements within the `<dependencies>` tag in the `pom.xml` file.  

### Dependency Version Management  
- **Maven (`<dependencyManagement>`):**  
  This POM manages most dependency versions by inheriting `spring-boot-starter-parent` (2.6.3). Custom versions are controlled via `<properties>` (e.g., `weixin-java-miniapp.version`) and explicit `<version>` tags. No independent `<dependencyManagement>` section is used.

## Project Structure

```text
weixin-java-miniapp-demo/
├── .editorconfig          # Editor configuration (standardization)
├── .gitignore            # Git ignore rules
├── README.md             # Project documentation
├── .travis.yml           # CI/CD configuration (Travis CI)
├── pom.xml               # Maven project configuration
├── commit.sh             # Git commit script (presumed)
├── src/
│   └── main/
│       ├── resources/
│       │   ├── application.yml.template  # Application configuration template
│       │   ├── tmp.png                  # Temporary image resource (presumed)
│       │   ├── templates/
│       │   │   └── error.html           # Error page template
│       │   └── META-INF/
│       │       └── additional-spring-configuration-metadata.json  # Spring configuration metadata
│       ├── java/
│       │   └── com/github/binarywang/demo/wx/miniapp/
│       │       ├── WxMaDemoApplication.java  # Spring Boot main class
│       │       ├── controller/
│       │       │   ├── WxMaMediaController.java  # WeChat media API controller
│       │       │   ├── WxMaUserController.java   # WeChat user API controller
│       │       │   └── WxPortalController.java   # WeChat entry controller
│       │       ├── utils/
│       │       │   └── JsonUtils.java           # JSON utility class
│       │       ├── error/
│       │       │   ├── ErrorController.java     # Global error handling
│       │       │   └── ErrorPageConfiguration.java  # Error page configuration
│       │       └── config/
│       │           ├── WxMaProperties.java      # WeChat configuration properties
│       │           ├── WxMaConfiguration.java  # WeChat configuration class
│       │           └── ResourcesConfig.java    # Resource file configuration
│       └── docker/
│           └── Dockerfile                # Container deployment configuration
└── .git/                                # Git version control directory (standard structure)
    ├── HEAD
    ├── index
    ├── packed-refs
    ├── description
    ├── config
    ├── refs/
    │   ├── heads/
    │   │   └── master
    │   ├── tags/
    │   └── remotes/
    │       └── origin/
    │           └── HEAD
    ├── logs/
    │   ├── HEAD
    │   └── refs/
    │       ├── heads/
    │       │   └── master
    │       └── remotes/
    │           └── origin/
    │               └── HEAD
    ├── objects/
    │   ├── info/
    │   └── pack/
    │       ├── pack-65cd097d42fa26229f5d18a7cb3bca5b2a574d53.pack
    │       └── pack-65cd097d42fa26229f5d18a7cb3bca5b2a574d53.idx
    ├── info/
    │   └── exclude
    └── hooks/
        ├── commit-msg.sample
        ├── push-to-checkout.sample
        └── ... (other Git hook sample files)

Naming convention: Standard Java package naming (com.github.organization.project), directories use lowercase + hyphen (e.g., miniapp-controller)

Layered structure:
1. Controller layer: controller package handles WeChat API requests
2. Utility layer: utils package provides JSON serialization support
3. Configuration layer: config package centralizes WeChat SDK configuration
4. Error handling: Dedicated error package for global exception handling

Extension design:
1. Externalized configuration via resources/application.yml.template
2. Dockerfile for containerized deployment
3. Travis CI configuration for automated builds
4. META-INF/additional-spring-configuration-metadata.json enhances IDE configuration hints
```