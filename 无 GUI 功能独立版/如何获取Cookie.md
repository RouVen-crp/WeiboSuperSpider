# 如何获取微博 Cookie - 图文教程

## ? 什么是 Cookie？

Cookie 是浏览器保存的登录凭证，用于证明你已经登录了微博账号。爬虫程序需要 Cookie 才能访问微博数据。

---

## ? 获取 Cookie 的步骤（推荐使用 Chrome 或 Edge 浏览器）

### 方法 1：使用 Chrome 浏览器（推荐）

#### 第一步：访问微博并登录

1. 打开 Chrome 浏览器
2. 访问 https://weibo.cn （注意是 cn 结尾）
3. 输入账号密码登录

#### 第二步：打开开发者工具

按 `F12` 键（或右键点击页面 → 检查）

#### 第三步：切换到 Network 标签

在开发者工具顶部找到 `Network`（网络）标签并点击

#### 第四步：刷新页面

按 `F5` 或点击浏览器刷新按钮

#### 第五步：查找 Cookie

1. 在 Network 标签的请求列表中，点击第一个请求（通常是 `weibo.cn` 或类似名称）
2. 在右侧面板找到 `Request Headers`（请求头）
3. 向下滚动找到 `Cookie:` 这一行
4. 点击 Cookie 值，按 `Ctrl+A` 全选，然后 `Ctrl+C` 复制

#### 第六步：保存 Cookie

**方法 A：创建.env 文件（推荐）**

1. 在 `无 GUI 功能独立版/` 目录下创建一个名为 `.env` 的文件（注意文件名以点开头）
2. 用记事本打开，粘贴以下内容：
   ```
   COOKIE=这里粘贴你复制的Cookie
   ```
3. 保存文件

**Windows 创建.env 文件的技巧：**

- 打开记事本
- 点击"另存为"
- 文件名输入：`.env`（包括前面的点）
- 文件类型选择"所有文件"
- 保存

**方法 B：设置环境变量**

Windows PowerShell：

```powershell
$env:COOKIE="这里粘贴你的Cookie"
```

---

## ? 方法 2：使用 Edge 浏览器

步骤与 Chrome 完全相同，只需将 Chrome 替换为 Edge 即可。

---

## ? 方法 3：使用 Firefox 浏览器

#### 步骤 1-3：同 Chrome

登录微博 → 按 F12 → 切换到"网络"标签

#### 步骤 4：刷新页面并找 Cookie

1. 刷新页面
2. 点击第一个请求
3. 在右侧找到"请求头"
4. 找到 Cookie 并复制

---

## ? 验证 Cookie 是否正确

创建一个简单的 Python 脚本测试：

```python
import requests

cookie = "你的Cookie"
headers = {'Cookie': cookie}
res = requests.get('https://weibo.cn/', headers=headers)

if '登录' in res.text or '新浪' in res.text[:100]:
    print("? Cookie无效或已过期")
else:
    print("? Cookie有效")
```

---

## ? Cookie 示例格式

正确的 Cookie 应该看起来像这样：

```
_T_WM=xxxxxxxxxxxxx; SUBP=0033WrSXqPxfM72wWs9jqgMF55529P9D9WWnO7iNxxxxxxxxxxx; SCF=xxxxxxxxxxxxx; SUB=_2A25KdDqDeRhGeB9I6lMV9CfKzzqIHXVopDsTrDV6PUJbkdAGLWz4kW1NS1234567890abcdefghijklmn; SSOLoginState=1234567890
```

**注意：**

- Cookie 是一长串文本，包含多个键值对
- 键值对之间用分号和空格分隔
- 不要有多余的换行或空格

---

## ?? 重要提示

### 1. Cookie 安全

- ? **不要**将 Cookie 分享给他人
- ? **不要**在公开场合展示你的 Cookie
- ? **不要**将包含 Cookie 的代码上传到 GitHub 等平台
- ? **建议**使用小号（非主账号）进行数据采集

### 2. Cookie 有效期

- Cookie 通常在 30 天后过期
- 如果更换设备或清除浏览器数据，Cookie 会失效
- 如果微博提示"请登录"，说明 Cookie 已过期，需要重新获取

### 3. 使用建议

- 使用`.env`文件保存 Cookie，不要直接写在代码中
- 将`.env`添加到`.gitignore`，避免意外上传
- 定期更新 Cookie（建议每 2 周更新一次）

---

## ? 常见问题

### Q1: 找不到 Cookie 这一行？

**解决方法：**

1. 确保已经登录微博
2. 刷新页面后立即查看 Network 标签
3. 如果请求太多，可以清空后再刷新
4. 确保点击的是最上面的第一个请求

### Q2: Cookie 太长，复制不完整？

**解决方法：**

- 右键点击 Cookie 值 → 选择"Copy value"
- 或者双击 Cookie 值，然后 Ctrl+A 全选

### Q3: Cookie 包含换行符怎么办？

**解决方法：**
复制后粘贴到.env 文件时，删除所有换行符，确保 Cookie 在同一行。

### Q4: .env 文件不起作用？

**检查清单：**

- [ ] 文件名是否为 `.env`（以点开头）
- [ ] 文件是否在 `无 GUI 功能独立版/` 目录下
- [ ] 格式是否为 `COOKIE=你的Cookie值`
- [ ] Cookie 值是否用引号包裹（不需要引号）
- [ ] Cookie 值是否完整（没有换行）

### Q5: 怎么判断 Cookie 是否过期？

运行程序时如果出现以下情况，说明 Cookie 已过期：

- 提示"COOKIE 为空"
- 提示"cookie 错误或已过期"
- 提示"无法访问微博页面"

**解决方法：**重新获取 Cookie

---

## ? 需要帮助？

如果按照本教程仍然无法获取 Cookie，可能的原因：

1. 微博登录状态异常 → 尝试退出后重新登录
2. 浏览器版本过旧 → 更新到最新版本
3. 浏览器扩展干扰 → 尝试在无痕模式下操作

---

## ? 下一步

Cookie 配置完成后，可以：

1. 运行测试脚本验证：`python test_deep_analyzer.py`
2. 开始分析微博：查看 [快速开始\_WeiboDeepAnalyzer.md](快速开始_WeiboDeepAnalyzer.md)
3. 阅读完整文档：查看 [WeiboDeepAnalyzer_README.md](WeiboDeepAnalyzer_README.md)

---

**祝你使用愉快！** ?
