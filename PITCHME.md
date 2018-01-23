## 值得买 用户画像系统实践

<br>

#### 基于千人千面业务

---

## Template Features

- Code Presenting |
- Repo Source, Static Blocks, GIST |
- Custom CSS Styling |
- Slideshow Background Image |
- Slide-specific Background Images |
- Custom Logo, TOC, and Footnotes |

---?code=src/go/server.go&lang=golang&title=Golang File

@[1,3-6](Present code found within any repo source file.)
@[8-18](Without ever leaving your slideshow.)
@[19-28](Using GitPitch code-presenting with (optional) annotations.)


---?gist=onetapbeyond/494e0fecaf0d6a2aa2acadfb8eb9d6e8&lang=scala&title=Scala GIST

@[23](You can even present code found within any GitHub GIST.)
@[41-53](GIST source code is beautifully rendered on any slide.)
@[57-62](And code-presenting works seamlessly for GIST too, both online and offline.)

---

### Template Versions

- #### [Base Template  @fa[external-link gp-download]](https://gitpitch.com/gitpitch/templates/netflix)
- #### [Code Maximized @fa[external-link gp-download]](https://gitpitch.com/gitpitch/templates/netflix?p=codemax)
- #### [Speaker Notes @fa[external-link gp-download]](https://gitpitch.com/gitpitch/templates/netflix?p=speaker)

---

### Template Versions

## 画像主要结果解释：

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

---?image=assets/image/gitpitch-audience.jpg

@title[Download this Template!]

### Get your presentation started!
### [Download this template @fa[external-link gp-download]](https://gitpitch.com/template/download/netflix)
