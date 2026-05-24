你可以把这个项目想成一个“博客网站工厂”：

routes 像前台接待，先看你要访问哪个网址
middlewares 像门卫，先判断你有没有登录、能不能进
controller 像业务员，接住请求，决定查什么数据、返回什么页面
requests 像表单质检员，检查你填的数据合不合格
model 像仓库管理员，负责从数据库拿数据、存数据
view 像页面模板，把数据渲染成 HTML 给浏览器看
所以这个项目的整体流程就是：

浏览器请求 -> 路由匹配 -> 中间件检查 -> 控制器处理 -> 模型查库/存库 -> 模板渲染 -> 返回页面

一、先用最通俗的话理解整个项目
这是一个“服务端渲染”的博客系统。

意思是：

用户在浏览器访问 /articles
Go 后端收到请吧求
后端去数据库查文章
再把数据塞进 HTML 模板
最后直接把完整页面返回给浏览器
所以它不是前后端分离项目，不是 Vue/React 调接口那种，而是 后端直接出页面。

二、项目目录，通俗解释
main.go
程序入口，像“总开关”。

它做了 4 件事：

读取配置
连接数据库
初始化模板
初始化路由并启动 HTTP 服务
你可以把它理解为：“把整个网站启动起来”。

bootstrap/
启动装配层，像“安装工”。

这里不写业务逻辑，只负责初始化：

db.go：连数据库、设置连接池、自动迁移表结构
route.go：创建路由，注册页面接口，挂静态资源
template.go：初始化模板文件系统
作用就是：把各个组件接起来。

config/
配置目录，像“项目设置中心”。

里面定义：

应用名、端口、URL
数据库配置
session 配置
分页配置
这些配置最终会被统一注册到 pkg/config 里，再通过 .env 读取。

通俗说就是：
这里决定项目怎么运行，不决定业务怎么处理。

routes/
路由目录，像“网站地图”。

例如：

/ -> 文章首页
/articles/{id} -> 文章详情
/auth/login -> 登录页
/auth/do-login -> 提交登录表单
/categories/{id} -> 某分类下的文章列表
这里的作用就是：告诉系统某个 URL 应该交给哪个控制器方法处理。

app/http/controllers/
控制器层，像“真正接待用户请求的人”。

控制器负责：

读取请求参数
调用 model 查数据
调用 request 做验证
判断成功还是失败
选择渲染哪个页面，或者重定向到哪
比如：

ArticlesController：文章相关
AuthController：登录注册相关
CategoriesController：分类相关
UserController：用户个人页
PagesController：静态页面和 404
这个目录是整个业务流程的核心调度者。

app/http/middlewares/
中间件层，像“门卫”。

请求进入控制器之前，先经过这里。

比如：

Auth：必须登录才能访问
Guest：只有未登录用户才能访问
StartSession：开启 session
RemoveTrailingSlash：去掉路径末尾的 /
它的作用是：在进入真正业务前，先做统一检查和预处理。

app/models/
模型层，像“数据管理员”。

这里定义了：

文章 Article
用户 User
分类 Category
同时还有 CRUD 方法，比如：

查某篇文章
查所有文章
按分类查文章
创建文章
更新文章
删除文章
它主要负责和数据库打交道。

通俗讲：

controller 决定“要查什么”，model 真正去数据库执行。

app/requests/
请求校验层，像“表单审核员”。

比如你发文章时，要检查：

标题不能为空
标题长度要合规
正文不能太短
注册时要检查：

用户名
邮箱
密码
确认密码
所以它的职责是：

专门负责检查用户提交的数据是否合法。

这样控制器就不会写太多乱七八糟的校验代码。

app/policies/
权限策略层，像“权限裁判”。

比如文章编辑、删除时，要判断：

当前登录用户是不是这篇文章的作者
如果不是，就不能修改。

所以它负责：
判断某个用户有没有权限对某条数据执行某个操作。

pkg/
公共工具层，像“工具箱”。

里面放的是全项目通用能力，不直接属于某个业务：

pkg/auth：登录态判断
pkg/session：session 操作
pkg/config：配置读取
pkg/view：模板渲染
pkg/route：命名路由、路由参数读取
pkg/model：GORM 数据库连接
pkg/logger：日志
pkg/password：密码处理
pkg/pagination：分页
pkg/flash：一次性提示消息
它的定位是：为业务层提供基础设施支持。

resources/views/
页面模板目录，像“页面模具”。

里面有：

articles/*.gohtml
auth/*.gohtml
categories/*.gohtml
layouts/*.gohtml
控制器把数据传进去，模板负责生成 HTML 页面。

public/
静态资源目录。

放前端资源：

CSS
JS
浏览器会直接访问这些文件。

tests/
测试目录。

目前项目里测试不多，主要放自动化测试代码。

三、这个项目有没有分层？
有，但不是那种很严格的企业级分层。

它现在的分层更像这样：
路由层：routes
中间件层：app/http/middlewares
控制器层：app/http/controllers
请求校验层：app/requests
模型/数据层：app/models
基础设施层：pkg
视图层：resources/views
它没有什么？
没有单独的 service 层
没有单独的 repository 层
也就是说：

业务逻辑大多直接写在 controller
数据操作大多直接写在 model
所以这是一个 MVC 风格项目，不是典型的：

controller -> service -> repository -> db

而更像：

controller -> model -> db

四、每个主要接口是怎么走的
下面我按“用户在浏览器里怎么操作”来给你讲。

1. 首页 / 文章列表流程
对应路由：

/
/articles
对应控制器：

ArticlesController.Index
核心流程：

用户访问首页 /
路由把请求交给 ArticlesController.Index
控制器调用 article.GetAll(r, 2)
model 去数据库查文章列表，并做分页
控制器把文章列表和分页数据传给模板
渲染 articles.index
浏览器看到文章列表页
通俗讲就是：

访问首页 -> 查所有文章 -> 套模板 -> 返回页面

2. 文章详情页流程
对应路由：

/articles/{id}
对应控制器：

ArticlesController.Show
核心流程：

用户访问 /articles/3
控制器先从 URL 里取出 id=3
调用 article.Get(id) 查询文章
如果数据库没找到，就返回 404
如果找到了，就再判断当前用户是否可以修改这篇文章
把文章数据传给模板 articles.show
返回详情页
通俗理解：

根据文章 ID 查一篇文章，然后显示出来。

3. 打开“新建文章”页面流程
对应路由：

/articles/create
中间件：

Auth
对应控制器：

ArticlesController.Create
流程：

用户访问 /articles/create
先经过 Auth 中间件
如果没登录：
拦截
提示“登录用户才能访问此页面”
重定向到首页
如果已登录：
进入控制器
直接渲染 articles.create
所以这个接口本质上就是：

先检查是否登录，登录了就给你看发文章表单。

4. 提交“新建文章”流程
对应路由：

POST /articles
中间件：

Auth
控制器：

ArticlesController.Store
这个流程非常典型，建议你记住。

具体过程
用户填写标题和正文，提交表单
请求进入 Store
控制器先拿当前登录用户
拼出一个 Article 对象：
Title
Body
UserID
调用 requests.ValidateArticleForm(_article) 做表单校验
如果校验失败：
不入库
把错误信息和原始表单数据回显到创建页面
如果校验成功：
调用 _article.Create() 存数据库
如果创建成功：
跳转到文章详情页
如果创建失败：
返回 500
用人话总结
填表 -> 检查表单 -> 合格就存数据库 -> 成功后跳到详情页，不合格就重新显示表单错误。

5. 打开“编辑文章”页面流程
对应路由：

/articles/{id}/edit
中间件：

Auth
控制器：

ArticlesController.Edit
流程：

用户访问某篇文章的编辑页
先经过登录检查
控制器根据 id 查文章
如果文章不存在，返回 404
如果文章存在，再检查权限：
当前用户是不是文章作者
如果没权限：
提示“未授权操作”
跳转首页
如果有权限：
渲染编辑页面
把原文章内容带到表单里
重点
这里不仅有“是否登录”，还有“是否是作者”。

所以它是两道门：

先登录
再判权限
6. 提交“更新文章”流程
对应路由：

POST /articles/{id}
中间件：

Auth
控制器：

ArticlesController.Update
流程和创建类似，但多了一步“先查原文”和“权限判断”。

具体过程
用户提交编辑后的标题和正文
控制器根据文章 ID 查原文章
如果没查到，返回 404
如果查到了，先判断当前用户能不能改
能改的话，把新表单数据覆盖到原文章对象里
调用 ValidateArticleForm 校验
校验失败：
重新显示编辑页
展示错误信息
校验成功：
调用 _article.Update() 更新数据库
更新成功：
跳回文章详情页
如果数据库里其实没变更：
提示“您没有做任何更改！”
用人话总结
先确认这篇文章是你的，再检查你填得对不对，对了才更新。

7. 删除文章流程
对应路由：

POST /articles/{id}/delete
中间件：

Auth
控制器：

ArticlesController.Delete
流程：

用户发起删除请求
先登录检查
根据文章 ID 查文章
检查当前用户是否有权限删
有权限则执行 _article.Delete()
删除成功后跳到文章列表页
删除失败返回错误
总结：

先查文章，再判权限，最后删除。

8. 打开注册页流程
对应路由：

/auth/register
中间件：

Guest
控制器：

AuthController.Register
流程：

用户访问注册页
先经过 Guest
如果已经登录：
不允许再访问注册页
提示并跳回首页
如果未登录：
显示注册页面
所以 Guest 和 Auth 是反过来的：

Auth：必须登录
Guest：必须未登录
9. 提交注册流程
对应路由：

POST /auth/do-register
控制器：

AuthController.DoRegister
流程：

用户填写用户名、邮箱、密码、确认密码
控制器把表单内容组装成 user.User
调用 requests.ValidateRegistrationForm(_user)
如果校验失败：
重新显示注册表单
回显错误和原值
如果校验成功：
调用 _user.Create()
创建成功后：
显示“注册成功”
自动登录
跳转首页
创建失败：
返回 500
这个接口本质是
注册 = 校验表单 + 创建用户 + 自动登录。

10. 打开登录页流程
对应路由：

/auth/login
中间件：

Guest
控制器：

AuthController.Login
流程很简单：

未登录用户访问登录页
显示登录表单
11. 提交登录流程
对应路由：

POST /auth/dologin
控制器：

AuthController.DoLogin
这是登录逻辑的核心。

流程
用户提交邮箱和密码
控制器调用 auth.Attempt(email, password)
Attempt 内部做了几件事：
先根据邮箱查用户
如果没找到，返回“账号不存在或密码错误”
找到了以后比对密码
如果密码不对，也返回“账号不存在或密码错误”
如果密码正确，把用户 ID 写入 session
控制器拿到结果：
成功：提示欢迎回来，跳首页
失败：回到登录页显示错误
最关键的一步
登录不是“把用户对象存在内存里”，而是：

把用户 ID 放到 session 里。

以后请求再来时，就通过 session 判断“这个人是不是已登录”。

12. 退出登录流程
对应路由：

POST /auth/logout
控制器：

AuthController.Logout
流程：

调用 auth.Logout()
本质是删除 session 里的 uid
提示“您已退出登录”
跳回首页
所以退出登录本质就是：

把 session 里的登录标记清掉。

13. 分类创建流程
对应路由：

/categories/create
POST /categories
控制器：

CategoriesController.Create
CategoriesController.Store
流程和创建文章很像：

先显示分类创建表单
用户提交分类名称
ValidateCategoryForm 校验
合法就入库
成功后跳首页
不合法就重新显示表单错误
14. 分类详情流程
对应路由：

/categories/{id}
控制器：

CategoriesController.Show
流程：

从 URL 读取分类 ID
查询分类信息
再根据分类 ID 查该分类下的文章
使用文章列表模板显示结果
也就是说：

分类页本质上就是“带筛选条件的文章列表页”。

15. 用户主页流程
对应路由：

/users/{id}
控制器：

UserController.Show
流程：

读取 URL 中的用户 ID
查该用户
查这个用户写过的所有文章
用文章列表模板显示这些文章
本质上是：

某个作者名下的文章集合页。

五、中间件到底在做什么
你做笔记时，一定要把中间件理解透。

Auth
作用：只有登录用户才能访问。

逻辑很简单：

调用 auth.Check()
看 session 里有没有 uid
没有就说明没登录
拦截并跳首页
Guest
作用：只有没登录的人才能访问。

比如登录页、注册页。

如果你已经登录了，还去访问登录页，就会被拦下来。

StartSession
作用：让当前请求可以使用 session。

因为登录、退出、flash 消息这些功能都依赖 session。

六、登录状态是怎么保存的
这个项目不是 JWT，而是 session 登录。

核心在 pkg/auth。

登录时
auth.Attempt() 成功后会：

把用户 ID 写入 session：uid
后续请求
中间件或业务代码可以通过：

auth.Check()：判断是否登录
auth.User()：获取当前登录用户
退出时
auth.Logout() 会：

删除 session 里的 uid
你可以记成：

session 里有 uid = 已登录，没有 uid = 未登录

七、这个项目为什么没有 service 层
这是很多初学者会问的。

现在的结构是
Controller 里既有流程控制
也有一部分业务逻辑
Model 里既定义数据结构
也写数据库操作
所以它更适合中小项目或教学项目，优点是：

简单
直观
好懂
缺点是：

控制器容易变胖
业务复杂后不容易维护
数据访问和业务规则分离得不够彻底
如果以后升级成更标准的结构
可以变成：

controller：只接请求、回响应
service：写业务逻辑
repository：专门操作数据库
但这个项目当前还没拆到这一步。

八、你做笔记时可以这样记“整体架构”
最短版
这个项目是一个 MVC 风格的 Go 单体博客系统，主要流程是：

路由 -> 中间件 -> 控制器 -> 请求校验 -> 模型 -> 视图

稍详细版
routes 负责 URL 到控制器的映射
middlewares 负责登录校验、访客校验、session 等通用处理
controllers 负责业务流程调度
requests 负责表单校验
models 负责数据库读写
views 负责页面渲染
pkg 负责提供通用基础能力