# 学生宿舍管理系统设计与实现

系统采用前后端分离架构，前端基于Vue.js框架构建用户界面，后端依托SpringBoot实现业务逻辑与数据交互。前端通过Axios组件调用RESTful API与后端通信，接口设计遵循OpenAPI规范，确保请求与响应的标准化。后端架构分层清晰，控制层负责接收请求并返回JSON数据，业务层封装宿舍分配、维修派单等核心逻辑，数据访问层通过MyBatis框架操作MySQL数据库。认证与授权机制采用JWT令牌，用户登录后生成Token并存储于客户端LocalStorage，每次请求携带Token进行权限校验，有效避免会话劫持风险。网络架构采用B/S模式，支持跨平台访问，用户可通过PC端或移动端浏览器直接使用系统。此外，通过Nginx实现反向代理与负载均衡，提升高并发场景下的系统稳定性。


# 部署

## Dormitory_backend（后端）

1.先将数据库导入本地MySQL数据库中再将

```
Dormitory_backend/src/main/resources/application.properties
```

2.路径下的个人数据库用户及密码

```
spring.datasource.username=root
spring.datasource.password=123456
```

3.启动main方法

## Dormitory_frontend（前端）

1.npm init

2.npm run serve

