# sign 学生签到系统

基于 Spring Boot 的课堂签到管理系统，面向 **学生 / 教师 / 管理员** 三种角色，覆盖从开课、签到、补签到统计与 Excel 导出的完整流程。

## 功能

- **学生**：登录、查看课程、签到 / 补签、查看签到历史、修改密码
- **教师**：登录、发起 / 结束签到与补签、查看签到明细、Excel 导入导出、修改密码
- **管理员**：登录、账号管理（学生 / 教师）、签到统计、Excel 导出、修改密码
- 签到状态：未开始 → 签到中 → 已完成 / 补签中；明细区分「正常签到 / 补签 / 未签到」
- 登录采用 ticket + Cookie 机制，按角色拦截校验，密码加盐存储

## 技术栈

| 层 | 技术 |
| --- | --- |
| 框架 | Spring Boot 1.5.9、Spring MVC、AOP |
| 持久层 | MyBatis + tk.mybatis 通用 Mapper、PageHelper、MySQL |
| 视图 | FreeMarker、Bootstrap、jQuery |
| 工具 | Apache POI（Excel）、Lombok、Fastjson、Commons Lang3 |
| 构建 | Maven（含 mybatis-generator 反向生成） |

## 快速开始

**1. 建库**

```bash
mysql -u root -p < src/main/resources/sql/sign.sql
```

脚本会创建 `sign` 数据库及 7 张表：`admin`、`teachers`、`students`、`courses`、`sign_detil`、`sign_statistical`、`login_ticket`。

**2. 配置数据库**

编辑 `src/main/resources/application-dev.yml`，填写本地 MySQL 地址与密码：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/sign?characterEncoding=utf-8
    username: root
    password: 你的密码
```

默认激活 `dev` 环境（见 `application.yml`）；`prod` 环境为线上配置，部署前请改为自己的数据库与凭据。

**3. 启动**

```bash
./mvnw spring-boot:run
```

服务端口 `8081`，浏览器访问 `http://localhost:8081/`。

**4. 账号**

账号需自行插入 `admin` / `teachers` / `students` 表，或由管理员在系统中添加。系统新增账号的默认初始密码为 `12345678`（见 `AccountPasswordConstant`），登录后请及时修改。

## 目录结构

```
src/main/java/com/showtime/sign
├── controller        # admin / student / teacher 三端控制器
├── service           # 业务接口与实现
├── mapper            # MyBatis Mapper
├── model             # entity / base
├── interceptor       # 登录与角色权限拦截
├── aspect            # AOP 切面
├── configuration     # Web / MyBatis 配置
├── exception/handle  # 异常与全局处理
├── constant/enums    # 常量与枚举
└── utils             # 工具类
src/main/resources
├── sql/sign.sql      # 建库建表脚本
├── mapper/*.xml      # SQL 映射
├── templates/        # FreeMarker 页面
└── static/           # css / js / img
```

## 说明

本项目为个人学习 / 练手项目，仅供学习参考。生产环境使用前请务必：更换 `prod` 中硬编码的数据库地址与明文密码、关闭 SQL 日志与敏感信息打印、加强密码策略。
