# WeiboDeepAnalyzer - 单条微博深度分析工具

## ? 功能介绍

WeiboDeepAnalyzer 是一个用于深度分析单条微博的 Python 工具，整合了现有微博爬虫代码的核心功能。

### 核心功能

1. **微博内容提取**

   - 完整微博文本（支持长微博展开）
   - 图片链接提取
   - 发布时间和来源
   - 基础互动数据（点赞、转发、评论数）

2. **评论深度爬取**

   - 所有评论内容
   - 评论者信息
   - 评论时间和点赞数
   - 支持分页爬取

3. **转发信息收集**

   - 转发者信息
   - 转发时的评论内容
   - 转发时间和点赞数

4. **互动统计分析**

   - 总体互动数据统计
   - Top 评论者/转发者排行
   - 平均内容长度分析

5. **多格式数据导出**
   - JSON 格式（完整数据）
   - CSV 格式（分类数据表）
   - 自动分文件夹存储

---

## ? 快速开始

### 1. 环境准备

确保已安装 Python 3.6+和依赖包：

```bash
pip install requests lxml pillow
```

### 2. 配置 Cookie

**方法 1：创建 .env 文件**

在`无 GUI 功能独立版/`目录下创建`.env`文件：

```
COOKIE=你的微博Cookie字符串
```

**方法 2：环境变量**

```bash
# Windows PowerShell
$env:COOKIE="你的Cookie字符串"

# Linux/Mac
export COOKIE="你的Cookie字符串"
```

**获取 Cookie 的步骤：**

1. 打开浏览器访问 https://weibo.cn
2. 登录你的微博账号
3. 按 F12 打开开发者工具
4. 切换到 Network（网络）标签
5. 刷新页面
6. 点击任意请求，找到 Request Headers 中的 Cookie
7. 复制整个 Cookie 字符串

Cookie 示例格式：

```
_T_WM=xxxxx; SUBP=xxxxx; SCF=xxxxx; SUB=xxxxx; SSOLoginState=xxxxx
```

### 3. 运行示例

```python
from WeiboDeepAnalyzer import WeiboDeepAnalyzer

# 基本用法：完整分析
analyzer = WeiboDeepAnalyzer(wid='QbelLys5Z')
analyzer.analyze()

# 限制爬取页数（节省时间）
analyzer = WeiboDeepAnalyzer(wid='QbelLys5Z')
analyzer.analyze(max_comment_pages=10, max_repost_pages=10)
```

---

## ? 详细使用说明

### 初始化参数

```python
WeiboDeepAnalyzer(
    wid='微博ID',              # 必需：微博ID或mid
    cookie=None,              # 可选：Cookie字符串（默认从环境读取）
    output_dir='weibo_analysis'  # 可选：输出目录名
)
```

### 完整分析流程

```python
analyzer = WeiboDeepAnalyzer(wid='QbelLys5Z')

# 方式1：一键完整分析
analyzer.analyze(
    max_comment_pages=None,  # None表示爬取所有评论
    max_repost_pages=None    # None表示爬取所有转发
)

# 方式2：分步执行
analyzer.get_weibo_content()           # 步骤1：提取微博内容
analyzer.get_all_comments(max_pages=5) # 步骤2：爬取评论（限5页）
analyzer.get_all_reposts(max_pages=5)  # 步骤3：爬取转发（限5页）
analyzer.generate_stats()              # 步骤4：生成统计
analyzer.export_json()                 # 步骤5：导出JSON
analyzer.export_csv()                  # 步骤6：导出CSV
```

### 获取微博 ID（wid）

#### 方法 1：从微博链接提取

- **手机端链接**：`https://m.weibo.cn/status/QbelLys5Z`

  - wid = `QbelLys5Z`

- **PC 端链接**：`https://weibo.com/1234567890/QbelLys5Z`

  - wid = `QbelLys5Z`

- **详情页链接**：`https://weibo.cn/comment/QbelLys5Z`
  - wid = `QbelLys5Z`

#### 方法 2：从数字 ID 转换

如果有数字形式的微博 ID，可以直接使用：

```python
analyzer = WeiboDeepAnalyzer(wid='4467107636950632')
```

---

## ? 输出文件说明

分析完成后，会在`weibo_analysis/{wid}/`目录下生成以下文件：

### 1. {wid}\_complete.json

完整的 JSON 格式数据，包含：

```json
{
  "weibo_content": {
    "wid": "微博ID",
    "content": "微博内容",
    "images": ["图片链接1", "图片链接2"],
    "like_count": 点赞数,
    "repost_count": 转发数,
    "comment_count": 评论数,
    ...
  },
  "comments": [
    {
      "comment_id": "评论ID",
      "commenter_name": "评论者昵称",
      "content": "评论内容",
      ...
    }
  ],
  "reposts": [...],
  "stats": {
    "interaction_stats": {...},
    "comment_stats": {...},
    "repost_stats": {...}
  }
}
```

### 2. {wid}\_weibo.csv

微博基本信息表格：
| 微博 ID | 作者 | 内容 | 图片数 | 发布时间 | 来源 | 点赞数 | 转发数 | 评论数 | 链接 |
|--------|------|------|--------|----------|------|--------|--------|--------|------|

### 3. {wid}\_comments.csv

评论详情表格：
| 评论 ID | 评论者 ID | 评论者昵称 | 评论内容 | 点赞数 | 发布时间 |
|--------|----------|------------|----------|--------|----------|

### 4. {wid}\_reposts.csv

转发详情表格：
| 转发者 ID | 转发者昵称 | 转发内容 | 点赞数 | 发布时间 |
|----------|------------|----------|--------|----------|

### 5. {wid}\_stats.csv

统计分析表格：
| 统计项 | 数值 |
|--------|------|
| 总点赞数 | xxx |
| 总转发数 | xxx |
| 总评论数 | xxx |

---

## ? 使用场景

### 场景 1：舆情分析

```python
# 分析某条热门微博的评论情绪和传播路径
analyzer = WeiboDeepAnalyzer(wid='热门微博ID')
analyzer.analyze(max_comment_pages=50)  # 限制评论页数节省时间

# 查看Top评论者
print(analyzer.stats['comment_stats']['top_commenters'])
```

### 场景 2：营销效果评估

```python
# 分析品牌微博的互动效果
analyzer = WeiboDeepAnalyzer(wid='品牌微博ID')
analyzer.analyze()

# 获取转发用户列表用于后续分析
reposts_df = pd.read_csv(f'weibo_analysis/{wid}/{wid}_reposts.csv')
```

### 场景 3：学术研究

```python
# 批量分析多条微博
wids = ['wid1', 'wid2', 'wid3']
for wid in wids:
    analyzer = WeiboDeepAnalyzer(wid=wid)
    analyzer.analyze(max_comment_pages=20, max_repost_pages=20)
    time.sleep(10)  # 避免请求过快
```

### 场景 4：竞品分析

```python
# 只获取评论数据进行文本分析
analyzer = WeiboDeepAnalyzer(wid='竞品微博ID')
analyzer.get_weibo_content()
comments = analyzer.get_all_comments(max_pages=30)

# 自定义处理评论数据
for comment in comments:
    print(f"{comment['commenter_name']}: {comment['content']}")
```

---

## ?? 高级配置

### 自定义请求延时

修改源码中的延时参数以适应不同网络环境：

```python
# 在get_all_comments方法中
time.sleep(random.uniform(1.5, 3))  # 默认1.5-3秒

# 在get_all_reposts方法中
time.sleep(random.uniform(1.5, 3))  # 默认1.5-3秒
```

### 请求重试次数

```python
# 在_request方法中
def _request(self, url, timeout=10, retry=3):  # retry参数控制重试次数
```

### 自定义输出目录

```python
analyzer = WeiboDeepAnalyzer(
    wid='QbelLys5Z',
    output_dir='custom_output'  # 自定义输出目录
)
```

---

## ?? 技术架构

### 代码重构说明

本工具整合了以下现有爬虫代码的核心功能：

1. **WeiboUserScrapy.py**

   - 复用：HTML 解析、时间解析、内容提取逻辑
   - 改造：从批量用户微博爬取改为单条微博深度提取

2. **WeiboRepostSpider.py**

   - 复用：转发页面解析逻辑
   - 改造：整合到统一的分析流程中

3. **WeiboCommentScrapy.py**

   - 复用：评论页面解析和用户信息提取
   - 改造：简化流程，去除多线程，改为顺序执行

4. **WeiboSuperCommentScrapy.py**
   - 参考：评论 ID 转换逻辑（mid2id）
   - 说明：未使用登录方式，保持使用 Cookie 的简单方式

### 核心模块

```
WeiboDeepAnalyzer
├── __init__()          # 初始化配置
├── _request()          # HTTP请求封装
├── _parse_html()       # HTML解析
├── _parse_time()       # 时间格式转换
├── get_weibo_content() # 微博内容提取
├── get_all_comments()  # 评论爬取
├── get_all_reposts()   # 转发爬取
├── generate_stats()    # 统计分析
├── export_json()       # JSON导出
├── export_csv()        # CSV导出
└── analyze()           # 完整分析流程
```

---

## ?? 注意事项

### Cookie 相关

1. **Cookie 有效期**：Cookie 会过期，如遇到登录提示需重新获取
2. **账号安全**：不要将 Cookie 分享给他人
3. **多账号**：建议使用小号进行数据采集

### 爬取限制

1. **频率限制**：请求过快可能被限制访问，建议：

   - 单次分析间隔 10 秒以上
   - 不要同时运行多个实例
   - 夜间时段爬取成功率更高

2. **数据量限制**：

   - 超热门微博（10 万+评论）建议限制页数
   - 使用`max_comment_pages`和`max_repost_pages`参数

3. **反爬策略**：
   - 代码已内置随机延时
   - 已关闭系统代理避免冲突
   - 建议关闭 VPN

### 错误处理

常见错误及解决方法：

1. **"COOKIE 为空"**

   - 检查.env 文件是否正确创建
   - 检查 Cookie 格式是否正确

2. **"无法访问微博页面"**

   - Cookie 已过期，需重新获取
   - 网络连接问题
   - 微博 ID 不存在或已删除

3. **"HTTP 302/403"**

   - 请求过快被限制，等待后重试
   - Cookie 失效

4. **解析失败**
   - 微博页面结构变化
   - 特殊格式微博（视频、文章等）

---

## ? 性能参考

基于实测数据（网络环境：100Mbps 宽带）：

| 数据量     | 评论页数 | 转发页数 | 预计耗时   | 输出大小 |
| ---------- | -------- | -------- | ---------- | -------- |
| 小型微博   | 10       | 10       | 1-2 分钟   | <1MB     |
| 中型微博   | 50       | 50       | 5-10 分钟  | 5-10MB   |
| 大型微博   | 100+     | 100+     | 20-30 分钟 | 20-50MB  |
| 超大型微博 | 500+     | 500+     | 1-2 小时   | 100MB+   |

**优化建议**：

- 对于超大型微博，建议分批次爬取
- 使用`max_pages`参数限制爬取范围
- 优先爬取最重要的数据（如评论）

---

## ? 与原有代码对比

### 原有代码特点

- **WeiboUserScrapy.py**：批量爬取用户所有微博
- **WeiboCommentScrapy.py**：单独爬取评论
- **WeiboRepostSpider.py**：单独爬取转发
- **分散**：功能分散在多个文件

### WeiboDeepAnalyzer 优势

? **功能整合**：一个类完成所有功能  
? **单条深度**：专注于单条微博的完整分析  
? **结构化输出**：统一的 JSON+CSV 格式  
? **统计分析**：内置互动数据分析  
? **易于使用**：一行代码完成完整分析  
? **可扩展**：模块化设计便于二次开发

---

## ? 贡献与反馈

### 问题反馈

如遇到问题，请提供：

1. 完整错误信息
2. 使用的微博 ID
3. Python 版本和依赖版本

### 功能建议

欢迎提出新功能需求，例如：

- 图片下载功能
- 视频链接提取
- 更多统计维度
- 数据可视化

---

## ? 许可说明

本工具仅供学习和研究使用，请遵守以下原则：

1. **合法使用**：仅爬取公开数据，不侵犯用户隐私
2. **合理频率**：不对微博服务器造成过大负担
3. **数据保护**：妥善保管爬取的数据
4. **商业使用**：如需商业使用请遵守微博用户协议

---

## ? 联系方式

- 基于原作者代码重构：inspurer(月小水长)
- 原作者 GitHub: https://github.com/inspurer
- 微信公众号: 月小水长(ID: inspurer)

---

## ? 更新日志

### v1.0.0 (2025-10-29)

- ? 初始版本发布
- ? 整合现有代码功能
- ? 实现单条微博深度分析
- ? 支持 JSON 和 CSV 双格式导出
- ? 内置统计分析功能
