# spring_forum
一个基于 Spring Boot 3.2.0 开发的现代化论坛系统，支持用户注册登录、帖子发布、回复、搜索等功能。

## 📋 项目简介

本项目是一个完整的论坛系统，从传统的 Servlet 架构重构为 Spring Boot 架构，采用了现代化的开发模式和最佳实践。系统支持用户注册、登录、发帖、回帖、编辑、删除等完整的论坛功能。

## 🚀 技术栈

- **框架**: Spring Boot 3.2.0
- **视图层**: JSP + JSTL
- **数据库**: MySQL 8.0+
- **构建工具**: Maven
- **Java版本**: JDK 17
- **安全**: BCrypt 密码加密
- **服务器**: 嵌入式 Tomcat

## ✨ 主要改进

### 1. 架构升级：从 Servlet 到 Spring Boot

#### 改进前（Servlet架构）

- 使用 `@WebServlet` 注解的传统 Servlet
- 手动管理数据库连接（`DBUtil`）
- 硬编码的配置信息
- WAR 包部署，需要外部 Tomcat

#### 改进后（Spring Boot架构）

- ✅ 使用 `@Controller` 和 `@GetMapping`/`@PostMapping` 注解
- ✅ Spring 依赖注入（`@Autowired`）
- ✅ 配置文件化管理（`application.properties`）
- ✅ JAR 包部署，内置嵌入式 Tomcat
- ✅ 自动配置，零 XML 配置

### 2. 分层架构优化

```
Controller层 → Service层 → DAO层 → Database
```

- **Controller层**: 处理 HTTP 请求，参数绑定，视图返回
- **Service层**: 业务逻辑处理，数据验证
- **DAO层**: 数据访问，SQL 执行
- **Model层**: 实体类定义

### 3. 依赖注入和组件管理

- ✅ 使用 `@Service`、`@Repository`、`@Controller` 注解
- ✅ Spring 管理的 `DataSource`，自动连接池管理
- ✅ 消除了手动资源管理代码

### 4. JSP 部署解决方案

**问题**: Spring Boot 在 JAR 模式下，JSP 文件无法直接通过 `InternalResourceViewResolver` 访问。

**解决方案**:

- 创建 `JspExtractor` 组件，应用启动时自动从 JAR 包中解压 JSP 文件到外部目录
- 通过 `WebServerFactoryCustomizer` 配置 Tomcat Context，将外部目录添加为资源路径
- 视图解析器可以正常访问解压后的 JSP 文件

### 5. 配置管理优化

- ✅ 使用 `application.properties` 统一管理配置
- ✅ 支持多环境配置（`application-prod.properties`）
- ✅ 数据库连接、端口、文件上传等配置外部化

### 6. 安全性增强

- ✅ 使用 BCrypt 进行密码加密存储
- ✅ Session 管理用户登录状态
- ✅ 文件上传类型验证

### 7. 代码质量提升

- ✅ 统一的异常处理
- ✅ 清晰的错误消息返回
- ✅ 日志记录（SLF4J + Logback）

## 🎯 功能特性

### 用户功能

- ✅ 用户注册（用户名、密码、邮箱验证）
- ✅ 用户登录（验证码支持）
- ✅ 用户退出登录
- ✅ 个人主页（查看自己发布的帖子）

### 帖子功能

- ✅ 发布帖子（支持标题、内容、图片上传）
- ✅ 编辑帖子（仅作者可编辑）
- ✅ 删除帖子（仅作者可删除）
- ✅ 帖子列表（分页、搜索）
- ✅ 帖子详情查看

### 回复功能

- ✅ 回复帖子
- ✅ 查看回复列表

### 其他功能

- ✅ 图片上传（支持 JPG/PNG 格式）
- ✅ 帖子搜索（按标题关键词）
- ✅ 验证码生成

## 📁 项目结构

```
spring_forum/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/lostandfound/
│   │   │       ├── config/              # 配置类
│   │   │       │   ├── DataSourceConfig.java      # 数据源配置
│   │   │       │   ├── JspExtractor.java         # JSP文件解压器
│   │   │       │   ├── JspViewResolverConfig.java # 视图解析器配置
│   │   │       │   └── WebMvcConfig.java          # MVC配置
│   │   │       ├── controller/          # 控制器层
│   │   │       │   ├── LoginController.java
│   │   │       │   ├── RegisterController.java
│   │   │       │   ├── ThreadPublishController.java
│   │   │       │   ├── ThreadListController.java
│   │   │       │   └── ...
│   │   │       ├── service/             # 服务层
│   │   │       │   ├── UserService.java
│   │   │       │   ├── ThreadService.java
│   │   │       │   └── ReplyService.java
│   │   │       ├── model/               # 数据模型和DAO
│   │   │       │   ├── User.java / UserDAO.java
│   │   │       │   ├── Thread.java / ThreadDAO.java
│   │   │       │   └── Reply.java / ReplyDAO.java
│   │   │       ├── util/                # 工具类
│   │   │       │   ├── DBUtil.java
│   │   │       │   └── StringUtil.java
│   │   │       └── SpringForumApplication.java  # 主启动类
│   │   ├── resources/
│   │   │   ├── application.properties         # 开发环境配置
│   │   │   ├── application-prod.properties      # 生产环境配置
│   │   │   └── META-INF/resources/              # JSP文件（打包到JAR）
│   │   │       ├── login.jsp
│   │   │       ├── register.jsp
│   │   │       ├── threadList.jsp
│   │   │       └── ...
│   │   └── webapp/
│   │       └── upload/                  # 上传文件目录
│   └── test/                            # 测试代码
├── pom.xml                              # Maven配置
└── README.md                            # 项目说明
```

## 🛠️ 快速开始

### 环境要求

- JDK 17+
- Maven 3.6+
- MySQL 8.0+

### 本地开发

1. **克隆项目**

   ```bash
   git clone <repository-url>
   cd spring_forum
   ```
2. **配置数据库**

   修改 `src/main/resources/application.properties`:

   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/forum_db?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
   spring.datasource.username=your_username
   spring.datasource.password=your_password
   ```
3. **创建数据库**

   ```sql
   CREATE DATABASE forum_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```

   然后执行数据库初始化脚本（需要根据项目实际情况创建表结构）。
4. **运行项目**

   ```bash
   mvn spring-boot:run
   ```

   或者打包后运行：

   ```bash
   mvn clean package
   java -jar target/JavaWebForum-1.0-SNAPSHOT.jar
   ```
5. **访问应用**

   打开浏览器访问: `http://localhost:8081`

## 📦 部署说明

### 生产环境部署

1. **打包项目**

   ```bash
   mvn clean package
   ```

   生成的文件: `target/JavaWebForum-1.0-SNAPSHOT.jar`
2. **上传到服务器**

   将 JAR 包上传到服务器，例如: `/www/wwwroot/forum/app/`
3. **创建必要目录**

   ```bash
   mkdir -p /www/wwwroot/forum/jsp      # JSP解压目录
   mkdir -p /www/wwwroot/forum/upload    # 文件上传目录
   mkdir -p /www/wwwroot/forum/logs      # 日志目录
   chmod 755 /www/wwwroot/forum/jsp
   chmod 755 /www/wwwroot/forum/upload
   ```
4. **配置生产环境**

   修改 `src/main/resources/application-prod.properties` 中的数据库配置：

   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/forum_db?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
   spring.datasource.username=forum_db
   spring.datasource.password=your_password
   jsp.extract.path=/www/wwwroot/forum/jsp
   ```
5. **启动应用**

   ```bash
   cd /www/wwwroot/forum/app
   nohup java -jar JavaWebForum-1.0-SNAPSHOT.jar --spring.profiles.active=prod > ../logs/app.log 2>&1 &
   ```
6. **查看日志**

   ```bash
   tail -f /www/wwwroot/forum/logs/app.log
   ```
7. **配置 Nginx 反向代理（可选）**

   在宝塔面板中：

   - 网站 → 添加站点
   - 设置反向代理到 `http://127.0.0.1:8081`
8. **开放端口**

   在宝塔面板中：

   - 安全 → 添加端口 8081（如果直接访问）
   - 或只开放 80/443（使用 Nginx）

### 使用 systemd 管理服务（推荐）

创建服务文件 `/etc/systemd/system/forum.service`:

```ini
[Unit]
Description=Spring Forum Application
After=syslog.target

[Service]
User=root
ExecStart=/usr/bin/java -jar /www/wwwroot/forum/app/JavaWebForum-1.0-SNAPSHOT.jar --spring.profiles.active=prod
SuccessExitStatus=143
StandardOutput=append:/www/wwwroot/forum/logs/app.log
StandardError=append:/www/wwwroot/forum/logs/app.log

[Install]
WantedBy=multi-user.target
```

管理命令：

```bash
# 启动服务
systemctl start forum

# 停止服务
systemctl stop forum

# 重启服务
systemctl restart forum

# 查看状态
systemctl status forum

# 开机自启
systemctl enable forum
```

## ⚙️ 配置说明

### application.properties

| 配置项                                        | 说明             | 默认值                 |
| --------------------------------------------- | ---------------- | ---------------------- |
| `server.port`                               | 服务器端口       | 8081                   |
| `spring.datasource.url`                     | 数据库连接URL    | -                      |
| `spring.datasource.username`                | 数据库用户名     | -                      |
| `spring.datasource.password`                | 数据库密码       | -                      |
| `jsp.extract.path`                          | JSP文件解压路径  | /www/wwwroot/forum/jsp |
| `spring.servlet.multipart.max-file-size`    | 最大文件上传大小 | 5MB                    |
| `spring.servlet.multipart.max-request-size` | 最大请求大小     | 10MB                   |

### 多环境配置

- **开发环境**: `application.properties`
- **生产环境**: `application-prod.properties`（通过 `--spring.profiles.active=prod` 激活）

## 🔧 技术亮点

### 1. JSP 在 JAR 模式下的解决方案

Spring Boot 官方文档明确说明 JSP 在 JAR 模式下支持有限。本项目通过以下方式解决了这个问题：

- **JspExtractor**: 应用启动时自动从 JAR 包中解压 JSP 文件到外部目录
- **Tomcat Context 配置**: 将外部目录添加为 Tomcat 的资源路径
- **视图解析器**: 配置 `InternalResourceViewResolver` 正确解析 JSP 文件

### 2. 依赖注入和组件管理

- 使用 Spring 的 `@Autowired` 进行依赖注入
- `@Service`、`@Repository`、`@Controller` 注解实现组件自动扫描
- Spring 管理的 `DataSource`，自动处理连接池

### 3. 配置外部化

- 所有配置项都在 `application.properties` 中
- 支持多环境配置
- 敏感信息（如数据库密码）可通过环境变量或外部配置文件管理

## 📝 数据库设计

主要数据表：

- **users**: 用户表（id, username, password, email, register_time）
- **threads**: 帖子表（id, title, content, user_id, publish_time, image_url）
- **replies**: 回复表（id, thread_id, user_id, content, reply_time）

