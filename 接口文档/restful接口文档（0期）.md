# nyasite_backend

## restful接口文档

未完成：/taglist /video /circle /circle manage /search /check /message /trending /history /marks

### 用户

#### api/user_status

GET方法

获取已登录用户自身的信息

传入：无

返回：

| Key    | Explain    | TYPE   |
| ------ | ---------- | ------ |
| name   | 用户名称       | String |
| userid | 用户id       | Number |
| mail   | 用户邮箱       | String |
| level  | 用户等级（详见等级） | Number |
| avatar | 用户头像链接     | String |

错误处理：如果用户没登录，返回代码401

#### api/user_status/:id

GET方法

根据id获取其他用户信息

传入：

| Param | Explain | TYPE   |
| ----- | ------- | ------ |
| id    | 用户id    | String |

返回：

| Key    | Explain | TYPE   |
| ------ | ------- | ------ |
| name   | 用户名称    | String |
| level  | 用户等级    | Number |
| avatar | 用户头像链接  | String |

错误处理：如果id格式不合适，返回代码400

#### api/search_users/:name

GET方法

根据用户名搜索用户信息

传入：

| Param | Explain | TYPE   |
| ----- | ------- | ------ |
| name  | 用户名称    | String |

返回：

| Key   | Explain | TYPE     |
| ----- | ------- | -------- |
| users | 用户信息集合  | Object[] |

相关类型说明：

users：

| Key    | Explain | TYPE   |
| ------ | ------- | ------ |
| id     | 用户id    | Number |
| avatar | 用户头像链接  | String |
| name   | 用户名称    | String |

用户等级：0以上

#### api/register

POST方法

用户注册

传入(表单)：

| Key      | Explain | TYPE   |
| -------- | ------- | ------ |
| username | 用户名     | String |
| passwd   | 密码      | String |
| email    | 电子邮件地址  | String |

返回：无

错误处理：如果username，passwd，email的格式不合适，返回代码400；如果username和email已经存在，返回409

#### api/login

POST方法

登录，并创建session cookie

传入(表单)：

| Key      | Explain | TYPE   |
| -------- | ------- | ------ |
| username | 用户名或者邮箱 | String |
| passwd   | 密码      | String |

错误处理：如果已经登录，返回代码403；如果username，passwd为空，返回400；如果用户不存在或者密码错误，返回401

#### api/logout

POST方法

登出，并更新cookie

传入：无

返回：无

#### api/refresh

GET方法

更新cookie的过期日期

传入：无

返回：无

错误处理：如果用户没登录，返回代码401；如果用户不存在或者用户密码不正确，返回400

#### api/clockin

POST方法

上传时区信息来改变用户等级和上次登陆时区信息

传入：

| Key      | Explain | TYPE   |
| -------- | ------- | ------ |
| timezone | 用户时区    | String |

返回：无

#### api/check_premission/:cid

用社团id检查社团的权限

POST方法

传入：

| Key | Explain | TYPE   |
| --- | ------- | ------ |
| cid | 社团id    | String |

返回：

| Key | Explain   | TYPE   |
| --- | --------- | ------ |
| 无   | 社团中用户的权限？ | String |

错误处理：如果用户不在社团中或者社团权限为0？，返回代码403

用户等级：0以上

#### api/new_tag

创建标签

POST方法

传入：

| Key     | Explain | TYPE   |
| ------- | ------- | ------ |
| tagname | 标签名称    | String |

返回：无

错误处理：如果用户没登录，返回代码401；如果标签已经存在，返回409

用户等级：10以上

### 回复

#### api/video_comment/:id/:pg

获取某个视频的所有回复

GET方法

传入：

| Param | Explain | TYPE   |
| ----- | ------- | ------ |
| id    | 视频id    | String |
| pg    | 获取回复的页数 | String |

返回：

| Key      | Explain | TYPE     |
| -------- | ------- | -------- |
| Body     | 回复内容    | Object[] |
| UserShow | 回复的作者信息 | Object[] |
| Count    | 回复总数    | Number   |

相关类型说明：

UserShow：

| Key    | Explain | TYPE   |
| ------ | ------- | ------ |
| id     | 用户id    | Number |
| name   | 用户名称    | String |
| avatar | 用户头像链接  | String |

Body：

| Key         | Explain          | TYPE     |
| ----------- | ---------------- | -------- |
| Vid         | 所属视频的id          | Number   |
| Text        | 内容               | String   |
| Author      | 作者               | Number   |
| Choose      | 未知               | Number   |
| CRdisplay   | 楼中楼的回复           | Object[] |
| Like        | Like 👍数量        | Number   |
| Dislike     | Dislike 👎点踩数量   | Number   |
| Smile       | Smile 😄数量       | Number   |
| Celebration | Celebration 🎉数量 | Number   |
| Confused    | Confused 😕数量    | Number   |
| Heart       | Heart ❤️数量       | Number   |
| Rocket      | Rocket 🚀数量      | Number   |
| Eyes        | Eyes 👀数量        | Number   |
| Model       | 回复元信息            | Object   |

CRdisplay：

| Key    | Explain        | TYPE    |
| ------ | -------------- | ------- |
| Vid    | 楼中楼上一层的id,自动生成 | Number  |
| Text   | 内容             | String  |
| Author | 作者             | Number  |
| Like   | 芝士点赞数量         | Number  |
| Like_c | 判断是否点赞         | Boolean |
| Model  | 回复元信息          | Object  |

Model：

| Key       | Explain | TYPE   |
| --------- | ------- | ------ |
| Id        | 回复id    | Number |
| CreatedAt | 创建时间    | Number |
| UpdatedAt | 更新时间    | Number |
| DeletedAt | 删除时间    | Number |

错误处理：如果pg小于1或者大于总页数，返回代码400；如果视频没有回复，返回500；如果回复个数为0，返回404

#### api/video_comment_reply/:id

获取某个回复和他的所有子回复

GET方法

传入：

| Param | Explain | TYPE   |
| ----- | ------- | ------ |
| id    | 回复id    | String |

返回：

| Key      | Explain  | TYPE     |
| -------- | -------- | -------- |
| Origin   | 回复内容     | Object   |
| Body     | 所有的子回复   | Object[] |
| UserShow | 子回复的作者信息 | Object[] |

相关类型说明：

UserShow：

| Key    | Explain | TYPE   |
| ------ | ------- | ------ |
| id     | 用户id    | Number |
| name   | 用户名称    | String |
| avatar | 用户头像链接  | String |

Origin：

| Key         | Explain          | TYPE     |
| ----------- | ---------------- | -------- |
| Vid         | 所属视频的id          | Number   |
| Text        | 内容               | String   |
| Author      | 作者               | Number   |
| Choose      | 用户选择的表情的编号，未选择为0 | Number   |
| CRdisplay   | 楼中楼的回复           | Object[] |
| Like        | Like 👍数量        | Number   |
| Dislike     | Dislike 👎点踩数量   | Number   |
| Smile       | Smile 😄数量       | Number   |
| Celebration | Celebration 🎉数量 | Number   |
| Confused    | Confused 😕数量    | Number   |
| Heart       | Heart ❤️数量       | Number   |
| Rocket      | Rocket 🚀数量      | Number   |
| Eyes        | Eyes 👀数量        | Number   |
| Model       | 回复元信息            | Object   |

CRdisplay：

| Key    | Explain        | TYPE    |
| ------ | -------------- | ------- |
| Vid    | 楼中楼上一层的id,自动生成 | Number  |
| Text   | 内容             | String  |
| Author | 作者             | Number  |
| Like   | 芝士点赞数量         | Number  |
| Like_c | 判断是否点赞         | Boolean |
| Model  | 回复元信息          | Object  |

Body：

| Key    | Explain        | TYPE    |
| ------ | -------------- | ------- |
| Vid    | 楼中楼上一层的id,自动生成 | Number  |
| Text   | 内容             | String  |
| Author | 作者             | Number  |
| Like   | 芝士点赞数量         | Number  |
| Like_c | 判断是否点赞         | Boolean |
| Model  | 回复元信息          | Object  |

Model：

| Key       | Explain | TYPE   |
| --------- | ------- | ------ |
| Id        | 回复id    | Number |
| CreatedAt | 创建时间    | Number |
| UpdatedAt | 更新时间    | Number |
| DeletedAt | 删除时间    | Number |

错误处理：如果回复获取失败，返回代码400；如果回复id不存在，返回404

#### api/add_video_comment

添加视频回复

POST方法

传入：

| Key  | Explain | TYPE   |
| ---- | ------- | ------ |
| vid  | 视频id    | String |
| text | 回复内容    | String |

返回：

| Key | Explain | TYPE   |
| --- | ------- | ------ |
| 无   | 回复id    | String |

用户等级：0以上

#### api/add_video_comment_reply

添加子回复

POST方法

传入：

| Key  | Explain | TYPE   |
| ---- | ------- | ------ |
| cid  | 回复id    | String |
| text | 子回复内容   | String |

返回：

| Key | Explain | TYPE   |
| --- | ------- | ------ |
| 无   | 子回复id   | String |

用户等级：0以上

#### api/click_comment_emoji

点击回复表情

POST方法

传入：

| Key   | Explain | TYPE   |
| ----- | ------- | ------ |
| emoji | 表情编号    | String |
| cid   | 回复id    | String |

返回：无

错误处理：如果emoji超出范围，返回代码400

#### api/click_commentreply_like

给子回复点赞或取消

POST方法

传入：

| Key  | Explain | TYPE   |
| ---- | ------- | ------ |
| crid | 子回复id   | String |

返回：无

#### api/add_video_tag

给视频添加标签

POST方法

传入：

| Key   | Explain | TYPE   |
| ----- | ------- | ------ |
| crid  | 子回复id   | String |
| tagid | 标签id    | String |

返回：无

用户等级：9以上

### 弹幕

#### api/get_bullets/:id

给视频添加标签

GET方法

传入：

| Param | Explain | TYPE   |
| ----- | ------- | ------ |
| id    | 视频id    | String |

返回（有弹幕时）：

| Key        | Explain       | TYPE     |
| ---------- | ------------- | -------- |
| items      | 弹幕信息          | Object[] |
| autoInsert | 是否显示用于发送弹幕的控件 | Boolean  |

返回（无弹幕时）：

| Key        | Explain       | TYPE    |
| ---------- | ------------- | ------- |
| autoInsert | 是否显示用于发送弹幕的控件 | Boolean |

返回值可以直接作为[Nplayer弹幕插件](https://nplayer.js.org/docs/ecosystem/danmaku)的Option

相关类型说明：

items

| Key   | Explain           | TYPE    |
| ----- | ----------------- | ------- |
| color | 颜色                | String  |
| text  | 文本                | String  |
| time  | 添加时间              | Number  |
| type  | 显示位置              | String  |
| isMe  | 是否为用户自己发送         | Boolean |
| force | 是否强制展示该弹幕(暂时没什么用) | String  |

错误处理：如果vid格式错误，返回代码400

#### api/add_video_bullet

给视频添加弹幕

POST方法

传入：

| Key   | Explain | TYPE   |
| ----- | ------- | ------ |
| vid   | 视频id    | String |
| text  | 文本      | String |
| time  | 添加时间    | String |
| color | 颜色      | String |
| type  | 显示位置    | String |

返回：无

错误处理：如果vid、time或position格式错误，返回代码400：如果插入失败，返回500

用户等级：0以上

### 更改用户信息

#### api/change_avatar

改变头像

POST方法

传入：

| Key    | Explain   | TYPE   |
| ------ | --------- | ------ |
| avatar | 头像在网络上的路径 | String |

返回：无

用户等级：1以上

#### api/change_name

改变用户名

POST方法

传入：

| Key  | Explain | TYPE   |
| ---- | ------- | ------ |
| name | 用户名     | String |

返回：无

用户等级：0以上

### token

#### api/get_PICUI_token

申请图床的临时token

GET方法

传入：无

返回：

| Key   | Explain | TYPE   |
| ----- | ------- | ------ |
| token | token   | String |

错误处理：？

用户等级：0以上

## video

##### author 子结构体

创作视频的社团

| Key      | Explain   |
| -------- | --------- |
| Name     | 社团名称      |
| Avatar   | 社团头像      |
| Relation | 访问者和社团的关系 |
| Id       | 社团id      |

##### videoReturn 子结构体

   返回的视频数据

| Key        | Explain             |
| ---------- | ------------------- |
| Id         | 视频id                |
| CoverPath  | 封面链接                |
| Title      | 标题                  |
| Author     | 创作的社团（一个author子结构体） |
| Views      | 播放量                 |
| Likes      | 点赞量                 |
| Marks      | 收藏量                 |
| SelfUpload | 是否为自己上传             |
| CreatedAt  | 视频的上传时间             |

#### api/get_video/:id

GET方法

获取视频信息

以伪静态链接形式传入查询视频的id（:id)

返回：

| Key         | Explain             |
| ----------- | ------------------- |
| title       | 视频的标题               |
| videoPath   | 视频的链接               |
| author      | 创作的社团（一个author子结构体） |
| creatTime   | 上传时间                |
| description | 简介                  |
| views       | 播放量                 |
| likes       | 点赞量                 |
| isLiked     | 是否已经点赞              |
| marks       | 收藏量                 |
| isMarked    | 是否已经收藏              |

#### api/upload_video

POST方法

上传视频

传入表单：

| Key         | Explain                 |
| ----------- | ----------------------- |
| author      | 创作社团的id                 |
| title       | 视频标题                    |
| description | 视频简介                    |
| cover       | 封面链接                    |
| tags        | 视频标签对应tagModel的id的array |

## 注册和登录的错误代码

### 注册

| 错误字符串            | 含义     |
| ---------------- | ------ |
| NameUsed         | 重复用户名  |
| EmailAddressUsed | 重复邮箱地址 |

### 登录

对登录而言200表示登录成功

| 错误代码 | 含义           |
| ---- | ------------ |
| 400  | 用户名或密码为空     |
| 401  | 用户名不正确或密码不存在 |
| 403  | 已经登录了        |

## 等级

h0-15

| 等级    | 含意           |
| ----- | ------------ |
| 0     | 什么都不能干的未答题用户 |
| 1     | 答完题的用户       |
| 10-14 | 普通管理员        |
| 15    | 超级管理员\开发者    |

## session

| 键      | 说明                          |
| ------ | --------------------------- |
| userid |                             |
| level  | 4bite权限4bite经验              |
| pwd-8  | 密码的最后8位,校验用,别问我为什么6位密码有8位数据 |

## 帖子类型（forum.kind)

| value | 含意         |
| ----- | ---------- |
| 0     | 官方通知区      |
| 1     | 用户反馈区      |
| 2     | 关闭的用户反馈区   |
| 3     | Thread贴    |
| 4     | 完结的Thread贴 |
| 5     | 资源帖        |

因为使用nodebb的原因,这部分暂时被废除了

## 表情编号

因为xorm的特性，表情记录从1开始，故每次调用都要-1

| emoji | 含意             |
| ----- | -------------- |
| 1     | Like 👍        |
| 2     | Dislike 👎     |
| 3     | Smile 😄       |
| 4     | Celebration 🎉 |
| 5     | Confused 😕    |
| 6     | Heart ❤️       |
| 7     | Rocket 🚀      |
| 8     | Eyes 👀        |

## 社团权限

MemberOfCircle{}.Permission

| value(int) | 含义         |
| ---------- | ---------- |
| 0          | 关注         |
| 1          | STAFF      |
| 2          | 创作者        |
| 3          | Maintainer |
| 4          | 社长         |
