## TA Recruitment System – 环境与基础配置说明

当前阶段，本仓库的 `README` 只说明**本地开发环境的配置步骤（JDK 17 + Tomcat 10 + Maven）**。  
后续会在文档中逐步补充需求、设计和分支规则等内容。

项目说明：

- 本仓库是一个使用 Maven 管理的 Java Servlet/JSP Web 应用，war 方式部署到本地 Tomcat。
- 数据全部存放在文本文件中（CSV 为主），不使用数据库。

---

### 项目结构概览（主要文件 / 文件夹）

- `pom.xml`  
  Maven 构建配置文件，定义了项目名称、打包方式（`war`）、Java 版本、依赖等。

- `src/main/java/`  
  存放全部 Java 源码。后续建议按包名划分子目录，例如：
  - `edu/bupt/ta/controller/`：Servlet 控制器；
  - `edu/bupt/ta/service/`：业务逻辑；
  - `edu/bupt/ta/storage/`：文件读写封装；
  - `edu/bupt/ta/model/`：实体类（User / Job / Application 等）。

- `src/main/webapp/`  
  Web 根目录，包含 JSP 页面和静态资源。
  - `index.jsp`：入口页面 / 首页。
  - `WEB-INF/web.xml`：Web 应用部署描述文件，配置 Servlet 映射、欢迎页等。

- `data/`  
  文本数据存放目录，当前包含示例数据文件：
  - `ta_users.csv`：示例 TA 用户数据；
  - `jobs.csv`：示例岗位数据；
  - `applications.csv`：示例申请记录。
  业务代码通过读写这些文件来实现“伪数据库”功能。

- `docs/`  
  项目文档目录：
  - `project-plan.md`：项目计划与迭代规划；
  - `requirements.md`：需求与用户故事摘要；
  - `architecture.md`：架构与分层设计说明。
  后续可以在此目录下继续补充测试、迭代记录等文件。

- `target/`（构建输出目录，默认不提交到 Git）  
  运行 `mvn clean package` 后自动生成，用于存放编译后的 class、打包出的 `ta-webapp.war` 等文件。  
  该目录属于构建产物，其他人可以通过 Maven 在本地重新生成，一般不需要也不应该提交到 GitHub。

---

### 一、系统环境配置（JDK 17 + Tomcat 10 + Maven）

#### 1. 安装并配置 JDK 17

1. 安装 JDK 17（例如解压到：`D:\Java\jdk-17.0.x`）。  
2. 配置系统环境变量（以 Windows 为例）：
   - 新建/修改系统变量 **`JAVA_HOME`**：
     - 值：`D:\Java\jdk-17.0.x`  
     - 注意：**不要在值后面加 `\bin`**。
   - 在系统变量 **`Path`** 中添加：
     - `%JAVA_HOME%\bin`
   - 如系统中已有其他 JDK 版本的路径（例如 JDK 21/22），可以将其移到下面或删除，确保 **JDK 17 的路径优先**。
3. 打开新的 PowerShell，验证：

```powershell
java -version
```

应看到输出类似：`java version "17.x.x"`。

---

#### 2. 安装并配置 Apache Tomcat 10

1. 下载 Tomcat 10：
   - 打开 `https://tomcat.apache.org/`，进入 **Tomcat 10** 下载页面；
   - 在 **Binary Distributions → Core** 选择 `64-bit Windows zip` 包。
2. 解压到固定目录，例如：
   - `D:\apache-tomcat-10.1.52`
3. 配置系统环境变量：
   - 新建系统变量 **`CATALINA_HOME`**：
     - 值：`D:\apache-tomcat-10.1.52`
   - 在系统变量 **`Path`** 中添加：
     - `%CATALINA_HOME%\bin`
4. 启动 Tomcat 并验证：

```powershell
cd D:\apache-tomcat-10.1.52\bin
startup.bat
```

在浏览器访问：

```text
http://localhost:8080
```

如果看到 Tomcat 欢迎页面，则说明 Tomcat 配置成功，并且正在使用当前系统的 JDK（建议为 17）。

---

#### 3. 安装并配置 Maven

1. 下载 Maven：
   - 打开 `https://maven.apache.org/download.cgi`
   - 下载 **Binary zip archive**（例如 `apache-maven-3.9.x-bin.zip`）。
2. 解压到固定目录，例如：
   - `D:\apache-maven-3.9.x`
3. 配置系统环境变量：
   - 新建系统变量 **`MAVEN_HOME`**：
     - 值：`D:\apache-maven-3.9.x`
   - 在系统变量 **`Path`** 中添加：
     - `%MAVEN_HOME%\bin`
4. 打开新的 PowerShell，验证：

```powershell
mvn -v
```

应看到 Maven 版本信息，并且 Java 版本为 17。

---