## 值得买 用户画像系统实践
<br>
#### 基于千人千面业务
<br>
###### 侯志伟/算法工程师

---

## 目录

- 用户画像简介 |
- 值得买的用户画像实践 |
- 用户画像应用与技术前沿 |
- 用户画像助力业务增长 |

---
@title[Introduction]
#### 用户画像简介
<br>

<left>挑战：在信息大爆炸时代，用户的个性化需求不断提高。
应对挑战：目前信息处理系统有两种工作模式。
第一种是“拉”模式，用户提交查询，搜索引擎返回搜索结果;
第二种是“推”模式，用户不需要显式提交任何查询和兴趣偏好，推荐系统通过自动化算法来进行“信息”推送。在信息智能时代，推荐系统已经成为互联网以及数据服务公司的核心技术产品。

推荐系统的终极目标就是能够理解用户，其中一个核心技术就是用户画像构建。</left>

+++
@title[Introduction]
#### 用户画像的几种表达
<br>

<br>
- 你的行为决定了你是谁<br>
- 在应用系统中，利用用户的相关数据(如文本、图片、行为等)来构建未知的用户属性特征等重要信息<br>
- 通过数据，得到用户的可量化的信息表示包括简单的属性特征(如年龄、性别等)以及复杂的模式特征(如网络隐含表示等)<br>


+++
@title[]
#### 用户画像-双刃剑
<center><big><big>"Big <del>brother</del> data is watching you……"</big></big></center>
<br>

@fa[arrow-down]

+++

网易云音乐：“你可能是个作家。”
-来自量化信息的抽象表达

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6g9ATVOTlFecxpqIYlzXWzW7L4iaQaKdEOHyOPZOwfBfGZ9PVibRQFD2w.gif" width = "600" height = "400" alt="dingding" align=center/>

+++
keep：“卸了吧，我快要keep不住你的体重了。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6S7Rn3ibwuAx9awMxR35Gicvw3qfTT3HU7icQ8vRtWfysRB12jVibAGooHQ.gif" width = "600" height = "400" alt="keep" align=center/>

+++
>钉钉：“以后别去用钉钉的公司了，不适合你。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6B5AEY5Glc1ickuFao9MX0Z88VAjv8bVFbzuyiaIGJkdFT2j7SrTibq9Lw.gif" width = "600" height = "400" alt="dingding" align=center/>

+++
>知乎：“谢邀。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6cTWKyA3Z27WSdPauUWOBKt9uj9VULAojIAsWoypwDyuh0pibEvmT4fA.gif" width = "600" height = "400" alt="dingding" align=center/>

>得到：“有中年焦虑的阅读障碍患者。”

<img src="http://oimnc6bmn.bkt.clouddn.com/JJbONXXaqJz3VvNhGoO0ibqhO3xBav8X6E1Hnib9gvbRWQC8ZGN54V1gFJ7Y34hEIGcBJo1VCItqJdW3FibsicicOjw.gif" width = "600" height = "400" alt="dingding" align=center/>

@title[Embed Images]

## Image Slides
## [ Inline ]
<span style="font-size:0.6em; color:gray">Press Down key for examples.</span> | <span style="font-size:0.6em; color:gray">See [GitPitch Wiki](https://github.com/gitpitch/gitpitch/wiki/Image-Slides) for details.</span>

@fa[arrow-down]

+++

#### Make A Visual Statement

<br>

Use inline images to lend
a *visual punch* to your slideshow presentations.


+++
@title[Private Investocat]

<span style="color:gray; font-size:0.7em">Inline Image at <b>Absolute URL</b></span>

![Image-Absolute](https://d1z75bzl1vljy2.cloudfront.net/kitchen-sink/octocat-privateinvestocat.jpg)


<span style="color:gray; font-size: 0.5em;">the <b>Private Investocat</b> by [jeejkang](https://github.com/jeejkang)</span>


+++
@title[Octocat De Los Muertos]

<span style="color:gray; font-size:0.7em">Inline Image at GitHub Repo <b>Relative URL</b></span>

![Image-Absolute](assets/octocat-de-los-muertos.jpg)

<span style="color:gray; font-size:0.5em">the <b>Octocat-De-Los-Muertos</b> by [cameronmcefee](https://github.com/cameronmcefee)</span>


+++
@title[Daftpunktocat]

<span style="color:gray; font-size:0.7em"><b>Animated GIFs</b> Work Too!</span>

![Image-Relative](https://d1z75bzl1vljy2.cloudfront.net/kitchen-sink/octocat-daftpunkocat.gif)

<span style="color:gray; font-size:0.5em">the <b>Daftpunktocat-Guy</b> by [jeejkang](https://github.com/jeejkang)</span>








| table | table_name | comment |
| ---- | :------: | :--------: |
| recommend.dw_cp_user_preference_30_days |  用户画像切片表 | 分区表，30天时间衰减后的切片 |
| recommend.dw_cp_user_preference_long_term|   用户画像长期表| 由切片表聚合而成（保留最近切片），写入最终 redis 里|
| recommend.tag_relation_user_preference| 用户拓展偏好| 基于用户特征的协同过滤得到的用户可能偏好，只包含三级品类 top50，非分区表，保留最新的切片|
| recommend.cold_start_user_preference| 新用户引导-临时画像表| 基于用户主动点击标签反馈得到的临时画像（8.7上线），hive 保存最近一个月数据，mysql 保留全量数据|
| recommend.cold_start_user_preference_guess| 新用户引导-临时画像拓展偏好表| 基于用户主动点击标签关联得到用户可能偏好，只包含1、2级品类（8.7上线），hive 保存最近一个月数据，mysql 保留全量数据|






---

[车联网用户画像系统 @fa[external-link gp-download]](https://www.slideshare.net/ssuserfe03a9/ss-76349106)



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
