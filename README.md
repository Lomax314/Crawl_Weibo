# Crawl_Weibo
## 文件含义
### 文件存储结构
```
|—— Data
    |—— User_data.txt
    |—— Rumor_data.txt
    |—— Comment_tree_info.txt
    |—— Comment_Data
        |—— rumor1_id.json
        |—— rumor2_id.json
        |—— rumor3_id.json
        ......
```
### 用户信息文件
1. 数据存储格式
```
user1_info_dict
user2_info_dict
user3_info_dict
......

```
2. 用户信息字典含义
> 注：在用户信息字典的所有键中，‘简介’（含）之后的键不一定能抓取到对应的值
``` python
user_info_dict = {
        'id': '',  # 用户id，值为int类型
        'screen_name': '',  # 用户昵称
        'follow_count': '',  # 关注数
        'followers_count': '',  # 粉丝数
        'statuses_count': '',  # 微博数
        'verified': '',  # 是否认证
        'verified_type': '',  # 认证类型。-1：未认证；0：黄V；1：红V（推测）；2：蓝V
        'verified_reason': '',  # 认证理由
        'gender': '',  # 性别。m：男；f：女
        'description': '',  # 简介
        'created_time': '',  # 注册时间/审核时间（对蓝V来说
        'location': '',  # 所在地
        'birthday': '',  # 生日
        'credit': ''  # 阳光信用：信用极好、信用较好、信用一般（信用较差）
    }
```
### 谣言数据文件
1. 数据存储格式
```
rumor1_data_dict
rumor2_data_dict
rumor3_data_dict
......

```
2. 谣言信息字典含义
``` python
rumor_data_dict = {
    'tip_time': '',  # 被举报时间
    'count_tips': '',  # 被举报次数
    'rumor_id': '',  # 谣言id，由谣言的发布时间戳+用户id的格式组成
    'rumor_url': '',  # 发布谣言的网址
    'rumor_user': '',  # 发布谣言的用户的id
    'rumor_time': '',  # 发布谣言的时间，格式为%Y-%m-%d %H:%M:%S
    'count_forward': '',  # 转发的数量
    'count_comment': '',  # 评论的数量
    'count_praised': '',  # 点赞的数量，均为int类型
    'rumor_content': ''  # 谣言内容，包括文本（包括超话和emoji）+照片+视频+文章，除文本外均以链接形式保存
}
```
### 评论树统计信息文件
1. 数据存储格式
> 注：源谣言内容本身为评论树真正的根节点，当只有谣言内容没有任何评论时，树的总节点个数为1，深度为1，叶子节点个数为1，平均深度也为1。所有评论树的的统计信息按平均深度从小到大排序
```
comment_tree1_info_dict
comment_tree2_info_dict
comment_tree3_info_dict
......

```
2. 评论树统计信息字典含义
``` python
comment_tree_info_dict = {
    'rumor_id': '',  # 谣言id
    'count_node': '',  # 树的节点个数
    'count_leaf': '',  # 树的叶子节点个数
    'depth': '',  # 树的（最大）深度
    'avg_depth': '',  # 树的平均深度
}
```
### 评论树数据文件
1.数据存储格式
> 注：考虑到源微博作为根节点的话，树最深最深为8层。评论树数据文件的文件名格式为：rumor_id.json，是一个json文件
```
{
  'rumor_id': [
    comment_data_dict1,
    comment_data_dict2,
    comment_data_dict3,
    ......
  ]
}

```
2. 评论数据字典含义
> 注：虽然键：'father_comment_id'的值可能为null，代表着这条评论是一级评论，从某种意义上说是ta所包含的二级评论的根节点。但其实源谣言内容本身才是整个回复树中真正的根节点，只是没在回复树数据文件中体现出来
```
comment_data_dict = {
    'comment_id': '',  # 评论id，格式为评论时间戳+用户id+后缀数字（后缀数字为四位数字，范围为0000-9999，后缀数字的作用是增加评论id的唯一性）
    'comment_time': '',  # 发布评论的时间，格式为%Y-%m-%d %H:%M。（注意，这里没有具体的秒数
    'user_id': '',  # 发布评论的用户id
    'comment_content': '',  # 评论内容，格式同谣言内容
    'father_comment_id': ''  # 父节点的评论id。若为None(显示为null)，则此条评论为一级评论，否则为二级评论
}
```
## To-do list
- [x] 用户信息的抓取
- [x] 谣言数据的抓取
- [x] 评论数据的抓取及评论树结构的建立
- [ ] 应对微博的反爬虫机制，大批量快速抓取用户信息（不太好弄，手里微博账号少
- [ ] 抓取非谣言数据，需要兼顾到谣言数据和非谣言数据之间领域的交叉

