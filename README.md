

## 项目概述

`ai-agent-spuer` 是一个基于Spring Boot的Java应用项目，采用模块化架构设计，遵循领域驱动设计（DDD）原则。

## 项目结构

项目采用多模块Maven结构，包含以下主要模块：

### 主要模块

1. **ai-agent-spuer-api**: API接口定义模块
    - 包含DTO数据传输对象
    - 定义对外暴露的接口

2. **ai-agent-spuer-app**: 应用启动模块
    - Spring Boot主应用入口
    - 配置文件和启动类 [Application](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\src\main\java\com\xin\Application.java#L6-L14)
    - 线程池配置、Guava缓存配置等

3. **ai-agent-spuer-domain**: 领域层模块
    - 核心业务逻辑实现
    - 包含adapter、model、service等子包
    - model下分为aggregate(聚合)、entity(实体)、valobj(值对象)

4. **ai-agent-spuer-trigger**: 触发层模块
    - HTTP接口控制器
    - 定时任务和事件监听器

5. **ai-agent-spuer-infrastructure**: 基础设施层模块
    - 数据库访问实现
    - 外部服务调用实现
    - Redis配置等

6. **ai-agent-spuer-types**: 公共类型模块
    - 枚举类型
    - 常量定义
    - 异常类定义

## 技术栈

- **核心框架**: Spring Boot 3.4.3
- **Java版本**: Java 17
- **数据库**: MySQL 8.0.28
- **ORM框架**: MyBatis 3.0.4
- **连接池**: HikariCP
- **JSON处理**: FastJSON 2.0.28
- **工具库**: Guava 32.1.3, Apache Commons Lang3 3.9
- **测试框架**: JUnit
- **安全认证**: JWT (java-jwt 4.4.0, jjwt 0.9.1)
- **日志框架**: Logback

## 核心配置

### 线程池配置
项目配置了自定义线程池，支持以下参数：
- 核心线程数: 20
- 最大线程数: 50
- 空闲时间: 5000ms
- 队列大小: 5000
- 拒绝策略: CallerRunsPolicy

### 数据库配置
```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://127.0.0.1:3306/xfg_frame_archetype
    driver-class-name: com.mysql.cj.jdbc.Driver
```


### 缓存配置
使用Guava Cache，设置3秒过期时间。

## 启动方式

1. 确保已安装Java 17和Maven
2. 在项目根目录执行: `mvn clean install`
3. 进入 `ai-agent-spuer-app` 模块
4. 执行: `mvn spring-boot:run` 或运行 [Application](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\src\main\java\com\xin\Application.java#L6-L14) 类

## Docker支持

项目包含Dockerfile和相关脚本：
- [Dockerfile](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\Dockerfile): 定义应用镜像构建过程
- [build.sh](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\build.sh): 本地镜像构建脚本
- [push.sh](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\push.sh): 镜像推送脚本（支持阿里云）
- [start.sh](file://D:\www\planetProject\ai-agent-spuer\docs\dev-ops\app\start.sh)/[stop.sh](file://D:\www\planetProject\ai-agent-spuer\docs\dev-ops\app\stop.sh): 容器启停脚本

## 配置文件

项目支持多种环境配置：
- [application-dev.yml](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\src\main\resources\application-dev.yml): 开发环境
- [application-test.yml](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\src\main\resources\application-test.yml): 测试环境
- [application-prod.yml](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-app\src\main\resources\application-prod.yml): 生产环境

默认激活开发环境配置。

## 领域模型设计

项目遵循DDD设计原则，包含以下包结构：
- **adapter**: 外部接口适配器
- **model**: 领域模型对象
    - aggregate: 聚合根
    - entity: 实体对象
    - valobj: 值对象
- **service**: 领域服务

## 异常处理

项目定义了统一的异常处理机制：
- [AppException](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-types\src\main\java\com\xin\types\exception\AppException.java#L5-L45): 自定义应用异常
- [ResponseCode](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-types\src\main\java\com\xin\types\enums\ResponseCode.java#L6-L19): 响应码枚举
- [Response](file://D:\www\planetProject\ai-agent-spuer\ai-agent-spuer-api\src\main\java\com\xin\api\response\Response.java#L9-L21): 统一响应格式

## 日志配置

使用Logback作为日志框架，配置了：
- 控制台输出
- INFO级别文件输出
- ERROR级别文件输出
- 异步日志处理
