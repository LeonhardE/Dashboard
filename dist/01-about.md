## About

**小河马点餐**是一个微信点餐小程序。它以微信为入口，使用手机进行点餐。小河马点餐相比传统点餐服务，除了支持传统的浏览、下单、付款的传统服务之外，还有以下几个特点：

1. 支持商家自定义推荐内容
2. 用户可以多人同时点餐
3. 商家可以不通过开发者更新菜品和推荐内容
4. 界面简单美观
5. 服务端 docker 化，可自动部署，自动重启
6. 前端高度组件化，低耦合，可复用

我们的项目的一部分已经公开，欢迎体验。由于体验需要用户名和密码，小程序需要有体验权限，请发送邮件到 [ECer23 的邮箱](370095872@qq.com)，我们会分享权限与你！

- [管理系统](http://111.230.31.38:8080/)

## Demo

（加载时间可能比较长，请耐心等候🙂。建议在 https://rookies-sysu.github.io/Dashboard/01-about 网站下查看）

小程序点单系统

![小程序点单GIF](http://ww4.sinaimg.cn/large/005JCcFJgy1fstkmg0fwsg30h30goe8b.gif)

商家管理系统

![商家管理系统GIF](http://ww1.sinaimg.cn/large/005JCcFJgy1fstj4zhufvg30yf0g8kjq.gif)

## 项目结构

项目主要分成三个部分

1. [Order-System-Frontend](https://github.com/rookies-sysu/Order-System-Frontend) 使用微信小程序开发的扫码点餐小程序，主要特性是
    - 支持单人和多人同时点餐
    - 用户可以在丰富的、实时更新的推荐系统中快速找到喜爱和热门的食物
    - 操作简单，人性化的UI设计
2. [Order-System-Backend](https://github.com/rookies-sysu/Order-System-Backend) 
3. [Management-System-Frontend](https://github.com/rookies-sysu/Management-System-Frontend) 使用 Vue 开发的商家管理系统，主要特性是
    - 商家可以增删改查菜品和推荐系统
    - 商家可以查看订单状况

## Iterations 迭代

### Iteration 3 (by Jun. 30)

#### 目标

1. 前端完成管理系统开发，实现菜品增删改查
2. 前后端完成对接，并完成多人点餐功能

#### Week 17 (Jun. 25 ~ Jun. 30)

后台

- [x] 解决数据库异步访问问题
- [x] 再次核对 API 文档，修正 API 设计
- [x] 从 nginx 端解决跨域问题
- [x] 打完收工

前端

- [x] 解决多人点餐
- [x] 实现菜品/推荐的增删改查，完成迭代
- [x] 合并小程序的分支和管理系统分支
- [x] 打完收工

#### Week 16 (Jun. 18 ~ Jun. 24)

后台

- [x] 尝试部署 Vue 生成的代码
- [x] 修改 nginx 的端口
- [x] 服务器端解决跨域问题

前端

- [x] 开发管理系统除了菜品增删改查以外的基本功能
- [x] 实现小程序的主要核心功能，剩多人点餐没有实现



#### Week 15 (Jun. 15 ~ Jun. 17)

停工一周

#### Week 14 (Jun. 4 ~ Jun. 10)

后台

- [x] 一键部署
- [x] 增加 nginx 支持

前端

- [x] 添加多人点餐内容

#### Week 13 (May. 28 ~ Jun. 3)

后台

- [x] docker 化
- [x] 重构数据库完成
- [x] 引入 redis

前端

- [] ~~重构前端代码~~

---

### Iteration 2

中间大家写作业去了，所以停工了一段时间。第二次迭代截止日期在五月月底

#### 目标

1. 前端完成第二版 UI
2. 前端更新 UI 第三版设计，包括多人同时点餐的功能支持
3. 后台按照前期文档中的领域模型等等来完成后台设计
4. 后台完成多人同时点餐的 API 设计

#### Week 12 (May. 21 ~ May. 27)

详细工作内容请见 [KANBAN - Week 12](https://github.com/orgs/rookies-sysu/projects/8)

后台

- [x] 重构代码，尝试用 `docker-compose` 构建
- [x] 测试数据库
- [x] 测试、修改 API 接口

前端

- [] ~~重构前端代码~~
- [x] 添加推荐详情页面
- [x] 合并个人分支，前端第二次迭代完成


#### Week 11 (May. 14 ~ May. 20)

详细工作内容请见 [KANBAN - Week 11](https://github.com/orgs/rookies-sysu/projects/7)

后台

- [x] 根据 API 文档开发
- [x] 重构数据库

前端

- [x] 修改推荐页面 UI 设计，增加推荐详情
- [x] 核对 API 细节
- [x] 实现购物车和菜单界面的数据共享

#### Week 10 (May. 7 ~ May. 13)

详细工作内容请见 [KANBAN - Week 10](https://github.com/orgs/rookies-sysu/projects/6)

- [x] 小程序今日推荐页面
- [x] 小程序菜单页面
- [x] 小程序订单详情页面
- [x] 后台 API 接口开放

---

### Itearation 1

因为我们小组成立于第三周，所以第一次迭代开始于第三周。

#### 目标

1. 前端初步完成项目原型，可以进行浏览菜品、选择、下单等服务
2. 后台初步完成数据库设计、基本的 API 开发
3. 尝试前端后台对接

#### Week 3 (Mar. 19 ~ Mar. 25)

详细的每天的工作完成情况在 [KANBAN - Week 3](https://github.com/orgs/rookies-sysu/projects/1)

**总的来说 ...**
- We chose to develop a simple version of our project as soon as possible. It should cover every basic demands, and run well in both front-end and back end. Also, data flow between front-end and back end should be complete.
- Analyze basic demands, finish [demand analysis](). (Demand analysis might update every week so if you want to trace the change of demand analysis please use version control tool). We also drew a flow chart for better perception [Link]().
- Confirm data flow and API format between front-end and back end.

**Front-end group**
- Determine [WeChat Applet](https://mp.weixin.qq.com/debug/wxadoc/introduction/index.html?t=2018323) as our front-end framework.
- Group members learn to use WeChat Applet.
- Finish a simple version of front-end UI design and development. In this week, front-end uses fake data to develop and debug.

**Back-end group**
- ... 

 
