# Twitter Data Collection

page_limit = 1

## 1. user_1000994344914972672_followers_cleaned.json

**数据模式**：用户关注者数组  
**格式**：JSON数组，包含多个用户对象  
- 每个对象代表一个关注用户1000994344914972672的账户
- 已添加TimescaleDB兼容的时间戳字段

**主要字段**：
- `user_id`: 用户唯一标识
- `user_name`: 用户显示名称
- `user_screen_name`: 用户Twitter句柄(@后的名称)
- `user_verified`: 账户是否认证
- `user_bio`: 用户简介
- `user_followers_count`/`user_following_count`/`user_tweets_count`: 用户统计数据
- `user_created_at`: 人类可读的账户创建时间
- `user_created_at_ts`: TimescaleDB兼容的账户创建时间戳(ISO-8601格式)
- `crawl_ts`: 数据抓取时间戳(ISO-8601格式)
- `user_location`: 用户位置
- `user_profile_image_url`: 个人头像URL
- `user_display_url`: 用户资料中的网站链接

## 2. user_1000994344914972672_followings_cleaned.json

**数据模式**：用户关注的账户数组  
**格式**：JSON数组，包含多个用户对象
- 每个对象代表用户1000994344914972672关注的一个账户
- 与followers格式一致，也添加了TimescaleDB兼容的时间字段

**主要字段**：
- 与followers数据结构完全相同
- 唯一区别是代表的关系方向不同(关注vs被关注)

## 3. user_1000994344914972672_replies_cleaned.json

**数据模式**：用户回复推文数组  
**格式**：JSON数组，包含多个推文对象
- 包括回复线程中所有推文，按线程分组
- 附加了线程上下文和互动信息

**主要字段**：
- 用户基本信息(`user_id`, `user_name`等)
- 推文内容字段:
  - `tweet_id`: 推文唯一标识
  - `text`: 推文内容
  - `created_at`: 人类可读的推文发布时间
  - `created_at_ts`: TimescaleDB兼容的推文时间戳
  - `crawl_ts`: 数据抓取时间戳
  - `language`: 推文语言
- 互动统计(likes, retweets, replies, quotes, views)
- 线程相关字段:
  - `thread_id`: 线程唯一标识
  - `position_in_thread`: 在线程中的位置
  - `is_root_tweet`: 是否为线程根推文
  - `reply_depth`: 回复嵌套深度
  - `thread_stats`: 线程统计信息，包含:
    - `reply_count`: 线程中回复数量
    - `target_user_replies`: 目标用户在该线程的回复数
    - `max_reply_depth`: 线程最大嵌套深度
- 回复关系(`in_reply_to_status_id`, `in_reply_to_user_id`等)
- 内容特征(`media_present`, `media_urls`, `hashtags`, `mentions`等)

## 4. user_1000994344914972672_tweet_cleaned.json

**数据模式**：用户发布的推文数组  
**格式**：JSON数组，包含多个推文对象
- 包括用户发布的所有类型推文(原创、引用、转发)
- 处理了嵌套内容(引用和转发)

**主要字段**：
- 用户基本信息字段(与replies相同)
- 推文内容字段(与replies相同)
- 引用和转发特有字段:
  - `quoted_status_summary`: 引用推文的摘要信息
  - `retweeted_status_summary`: 转发推文的摘要信息
  - `is_quote_status`: 是否为引用推文
  - `is_reply`: 是否为回复
- 内容特征与互动统计(与replies相同)
- 时间戳字段(与replies相同)

## 5. token_BTC_tweets.json (代币示例)

**数据模式**：关于比特币(BTC)的推文数组  
**格式**：JSON数组，包含多个推文对象

**主要字段**：
- 用户信息字段(与其他数据集类似)
- 推文内容字段:
  - `tweet_id`: 推文唯一标识
  - `text`: 推文内容
  - `created_at`: 推文发布时间
  - `created_at_ts`: 推文时间戳
  - `language`: 推文语言
- 互动统计:
  - `like_count`: 点赞数
  - `reply_count`: 回复数
  - `retweet_count`: 转发数
  - `quote_count`: 引用数
  - `view_count`: 浏览数
- 媒体内容:
  - `media_present`: 是否包含媒体
  - `media_urls`: 媒体文件URL数组
- 内容特征:
  - `hashtags`: 标签数组
  - `mentions`: 提及用户数组
  - `source`: 推文来源
- 推文关系:
  - `is_reply`: 是否为回复
  - `quoted_tweet_id`: 引用的推文ID
  - `conversation_id`: 对话ID
  - `original_tweet_id`: 原始推文ID
  - `in_reply_to_status_id`: 回复的推文ID
  - `in_reply_to_user_id`: 回复的用户ID
  - `in_reply_to_screen_name`: 回复的用户屏幕名
  - `is_quote_status`: 是否为引用推文
  - `possibly_sensitive`: 是否可能敏感
  - `quoted_status_summary`: 引用推文的摘要
  - `retweeted_status_summary`: 转发推文的摘要

