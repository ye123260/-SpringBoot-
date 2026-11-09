# 社区老年助餐预约管理系统

基于 Java 17、Spring Boot 2.7、JDBC、MySQL/H2 和原生 HTML/CSS/JavaScript 的课程项目。包含普通用户、商家、骑手、社区工作人员、社区管理者及管理员功能。支付和提现为模拟业务。

## 目录

- `src/main/java`：后端业务、接口和权限检查。
- `src/main/resources/static`：前端页面和样式。
- `src/main/resources/schema.sql`：通用建表定义，不含真实业务数据。
- `src/test/java`：自动化测试；`Fixture@9Ab` 是仅用于测试的公开夹具值，不能作为部署密码。
- `.env.example`：配置项说明，不含实际密码；Spring Boot 不会自动加载该文件。

## 本地构建

先安装 JDK 17 和 Maven，使用以下命令：

```shell
mvn -Dmaven.compiler.release=17 -Dmaven.compiler.testRelease=17 verify
java -jar target/meal-reservation-1.0.0.jar
```

默认只绑定本机回环地址，使用 H2 本地数据库，关闭示例账户初始化。首次运行空库没有可登录账户。数据和照片密钥在运行时写入 `data/`，该目录禁止提交。

需要本机演示时，在启动前自行设置环境变量：

```powershell
$env:MEAL_SEED = 'true'
$env:MEAL_SEED_PASSWORD = Read-Host '输入自行设置的初始化密码'
java -jar target/meal-reservation-1.0.0.jar
```

仅当账户表为空时才创建虚构示例数据，用户名包含 `admin`、`manager`、`merchant`、`courier`、`user`、`staff` 等；初始密码由你设置，不写入源码。示例角色共用初始化密码，只适合隔离的本机演示。已有账户时不会重置密码。初始化后移除环境中的初始化密码并关闭 MEAL_SEED。

测试通过隔离的内存数据库和测试属性显式开启示例数据，不依赖真实数据库或实际密码。

## MySQL 与 HTTPS

切换 MySQL 时设置 `SPRING_PROFILES_ACTIVE=mysql` 以及 `DB_HOST`、`DB_PORT`、`DB_NAME`、`DB_USER`、`DB_PASSWORD`。不要把真实值写进源码。数据库和 HTTPS 配置模板位于 `src/main/resources/application-*.properties`；启用 HTTPS 需另行配置证书和相关环境变量。

## 发布范围与限制

此目录只提供源码、测试、构建配置和通用说明，不包含原环境配置、数据库、照片、密钥、备份、运行日志、历史部署脚本、文档截图、Git 历史或构建产物。

保留的 `127.0.0.1` 是标准本机回环地址，不是任何人的公网 IP。业务接口路径和通用端口属于软件配置，公开源码无法隐藏这些信息。不要把“移除个人信息”理解为完成全面漏洞审计或保证无法被攻击。

`.gitignore` 只帮助排除未跟踪文件。提交前检查 `git status --short` 与 `git diff --cached`。GitHub 的账户、提交作者姓名邮箱、操作记录和后续自行添加的内容不属于本源码副本的匿名化范围。提交作者信息应由发布者在本地自行选择配置。
