## 值得买 用户画像系统实践
<br>
#### 基于千人千面业务
<br>
###### 侯志伟/算法工程师

---

## Template Features

- Code Presenting |
- Repo Source, Static Blocks, GIST |
- Custom CSS Styling |
- Slideshow Background Image |
- Slide-specific Background Images |
- Custom Logo, TOC, and Footnotes |

---
#### 前言
<br>

在信息大数据时代，用户的个性化需求不断提高。如何帮助用户有效获取所需要的信息，有力改善信息超载的问题，是数据科研工作者的主要研究方向，同时，对信息系统的智能度也是一个挑战。

目前信息处理系统有两种工作模式。第一种是“拉”模式，用户提交查询，搜索引擎返回搜索结果;第二种是“推”模式，用户不需要显式提交任何查询和兴趣偏好，推荐系统通过自动化算法来进行“信息”推送。在信息智能时代，推荐系统已经成为互联网以及数据服务公司的核心技术产品。

推荐系统的终极目标就是能够理解用户，其中一个核心技术就是用户画像构建。

---
#### 用户画像的几种表达
<br>

<br>
- 你的行为决定了你是谁<br>
- 在应用系统中，利用用户的相关数据(如文本、图片、行为等)来构建未知的用户属性特征等重要信息<br>
- 通过数据，得到用户的可量化的信息表示包括简单的属性特征(如年龄、性别等)以及复杂的模式特征(如网络隐含表示等)<br>


---
#### 用户画像-双刃剑
<center><big><big>"Big <del>brother</del> data is watching you……"</big></big></center>
<br>

---
>网易云音乐：“你可能是个作家。”
-来自量化信息的抽象表达

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6g9ATVOTlFecxpqIYlzXWzW7L4iaQaKdEOHyOPZOwfBfGZ9PVibRQFD2w.gif" width = "600" height = "400" alt="dingding" align=center/>

>keep：“卸了吧，我快要keep不住你的体重了。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6S7Rn3ibwuAx9awMxR35Gicvw3qfTT3HU7icQ8vRtWfysRB12jVibAGooHQ.gif" width = "600" height = "400" alt="keep" align=center/>


>钉钉：“以后别去用钉钉的公司了，不适合你。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6B5AEY5Glc1ickuFao9MX0Z88VAjv8bVFbzuyiaIGJkdFT2j7SrTibq9Lw.gif" width = "600" height = "400" alt="dingding" align=center/>


>知乎：“谢邀。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6cTWKyA3Z27WSdPauUWOBKt9uj9VULAojIAsWoypwDyuh0pibEvmT4fA.gif" width = "600" height = "400" alt="dingding" align=center/>

>得到：“有中年焦虑的阅读障碍患者。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6E1Hnib9gvbRWQC8ZGN54V1gFJ7Y34hEIGcBJo1VCItqJdW3FibsicicOjw.gif" width = "600" height = "400" alt="dingding" align=center/>


















---

[车联网用户画像系统 @fa[external-link gp-download]](https://www.slideshare.net/ssuserfe03a9/ss-76349106)




### Template Versions

画像主要结果解释：

| table          | table_name      | comment |
| -------------- | -------- | --------------------- |
| recommend.dw_cp_user_preference_30_days |  用户画像切片表 | 分区表，30天时间衰减后的切片 |
| recommend.dw_cp_user_preference_long_term|   用户画像长期表| 由切片表聚合而成（保留最近切片），写入最终 redis 里|
| recommend.tag_relation_user_preference| 用户拓展偏好| 基于用户特征的协同过滤得到的用户可能偏好，只包含三级品类 top50，非分区表，保留最新的切片|
| recommend.cold_start_user_preference| 新用户引导-临时画像表| 基于用户主动点击标签反馈得到的临时画像（8.7上线），hive 保存最近一个月数据，mysql 保留全量数据|
| recommend.cold_start_user_preference_guess| 新用户引导-临时画像拓展偏好表| 基于用户主动点击标签关联得到用户可能偏好，只包含1、2级品类（8.7上线），hive 保存最近一个月数据，mysql 保留全量数据|

表名：recommend.dw_cp_user_preference_30_days

| col_name          | data_type      | comment |
| -------------- | -------- | --------------------- |
| user_proxy_key	| string	| 代理键（三类 id 中的一个，优先级 user_id，imei，device_id）|
| tag_id	| string	| 标签 id（前缀 cate，brand，tag 表示标签的类型，后面的 id 为对应站内库的 id）|
| user_tag_weight| 	decimal(7,6)| 用户对该标签的偏好值|
| ds	| string| 	时间分区字段，格式为"2018-01-01"|

表名：recommend.dw_cp_user_preference_long_term

| col_name          | data_type      | comment |
| -------------- | -------- | --------------------- |
| user_proxy_key	| string	| 代理键（三类 id 中的一个，优先级 user_id，imei，device_id）|
| tag_id	| string	| 标签 id（前缀 cate，brand，tag 表示标签的类型，后面的 id 为对应站内库的 id）|
| user_tag_weight| 	decimal(7,6)| 用户对该标签的偏好值|
| ds	| string| 聚合数据来源分区，格式为"2018-01-01"|

表名：recommend.tag_relation_user_preference

| col_name          | data_type      | comment |
| -------------- | -------- | --------------------- |
| user_proxy_key	| string	| 代理键（三类 id 中的一个，优先级 user_id，imei，device_id）|
| tag_id	| string	| 标签 id（前缀 cate，brand，tag 表示标签的类型，后面的 id 为对应站内库的 id）|
| user_tag_weight| 	decimal(7,6)| 用户对该标签的偏好值|

表名：recommend.cold_start_user_preference

| col_name          | data_type      | comment |
| -------------- | -------- | --------------------- |
| user_proxy_key	| string	| 代理键（三类 id 中的一个，优先级 user_id，imei，device_id）|
| id	| int	| 标签 id（对应品类、品牌站内库的 id）|
| weight| 	decimal(7,6)| 用户对该标签的偏好值|
| class| 	string| 标签类型，cate、brand 中的一个|

表名：recommend.cold_start_user_preference_guess

| col_name          | data_type      | comment |
| -------------- | -------- | --------------------- |
| user_proxy_key	| string	| 代理键（三类 id 中的一个，优先级 user_id，imei，device_id）|
| id	| int	| 标签 id（对应品类 id）|
| weight| 	decimal(7,6)| 用户对该标签的偏好值|
---

### Questions?

<br>

@fa[twitter gp-contact](@gitpitch)

@fa[github gp-contact](gitpitch)

@fa[medium gp-contact](@gitpitch)

### Template Versions

[Speaker Notes @fa[external-link gp-download]](https://gitpitch.com/gitpitch/templates/netflix?p=speaker)

---

---?code=src/go/server.go&lang=golang&title=Golang File

@[1,3-6](Present code found within any repo source file.)
@[8-18](Without ever leaving your slideshow.)
@[19-28](Using GitPitch code-presenting with (optional) annotations.)


---?gist=onetapbeyond/494e0fecaf0d6a2aa2acadfb8eb9d6e8&lang=scala&title=Scala GIST

@[23](You can even present code found within any GitHub GIST.)
@[41-53](GIST source code is beautifully rendered on any slide.)
@[57-62](And code-presenting works seamlessly for GIST too, both online and offline.)
---?image=assets/image/gitpitch-audience.jpg

@title[Download this Template!]

### Get your presentation started!
### [Download this template @fa[external-link gp-download]](https://gitpitch.com/template/download/netflix)
