# 苍穹外卖 · 后端

基于黑马程序员《苍穹外卖》课程的学习实践项目。

> **说明**：初始工程结构与基础代码来自课程配套资料，本人在此基础上逐步实现各业务模块。
> 自己完成的改动均记录在提交历史中，便于区分。

---

## 技术栈

| 分类 | 技术 | 版本 |
|---|---|---|
| 框架 | Spring Boot | 2.7.3 |
| 持久层 | MyBatis（spring-boot-starter） | 2.2.0 |
| 数据库 | MySQL | 8.0 |
| 连接池 | Druid | 1.2.1 |
| 认证 | JWT（jjwt） | 0.9.1 |
| 接口文档 | knife4j（Swagger） | 3.0.2 |
| 工具 | Lombok | 1.18.20 |
| 构建 | Maven（多模块） | 3.9+ |
| 运行环境 | JDK | 8 / 11 |

## 模块划分

```
sky-take-out
├── sky-common    公共模块：常量、异常、工具类、统一返回结果、JWT 工具
├── sky-pojo      数据模型：Entity（对应表）、DTO（接收参数）、VO（返回视图）
└── sky-server    业务模块：Controller / Service / Mapper、配置、拦截器
```

分层的职责边界：

- **Controller** —— 只做参数接收与响应封装，不写业务逻辑
- **Service** —— 业务规则、事务、多个 Mapper 的编排
- **Mapper** —— 只负责 SQL，不含判断

## 当前进度

- [x] 多模块 Maven 工程搭建
- [x] 员工登录（JWT 签发，密码 MD5 校验）
- [x] 员工退出
- [ ] 员工管理（新增 / 分页查询 / 启用禁用 / 编辑）
- [ ] 分类管理
- [ ] 菜品管理
- [ ] 套餐管理
- [ ] 店铺营业状态
- [ ] 微信登录与用户端接口
- [ ] 购物车 / 订单 / 支付
- [ ] 数据统计与报表导出

## 如何运行

### 1. 准备数据库

```sql
CREATE DATABASE sky_take_out DEFAULT CHARACTER SET utf8mb4;
```

然后导入课程资料中的建表脚本（`资料/数据库/*.sql`）。

### 2. 修改数据源

编辑 `sky-server/src/main/resources/application-dev.yml`：

```yaml
sky:
  datasource:
    host: localhost
    port: 3306
    database: sky_take_out
    username: root
    password: 你的密码
```

### 3. 启动

```bash
# 在项目根目录
mvn clean install -DskipTests
mvn spring-boot:run -pl sky-server
```

或直接在 IDEA 中运行 `SkyApplication`。

### 4. 访问

- 服务地址：http://localhost:8080
- 接口文档：http://localhost:8080/doc.html

## 接口示例

| 方法 | 路径 | 说明 |
|---|---|---|
| POST | `/admin/employee/login` | 员工登录，返回 JWT |
| POST | `/admin/employee/logout` | 员工退出 |

登录后需在请求头携带 token：

```
token: <登录接口返回的 jwt>
```

## 关于本项目

这是一个**学习项目**，用于理解前后端分离架构下的后端开发流程：
需求分析 → 接口设计 → 数据库建模 → 分层编码 → 接口联调。

不是生产级代码，但每一层都按规范组织，便于后续扩展。
