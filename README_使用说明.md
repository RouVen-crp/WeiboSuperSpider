# 微博评论爬虫使用说明

### 运行爬虫

#### 1. WeiboCommentScrapy (基础评论爬虫)

```python
python "无 GUI 功能独立版/WeiboCommentScrapy(no longer maintained).py"
```

**使用前需要修改：**

- 第 32 行：将 `'''换成你自己的 cookie'''` 替换为真实的微博 cookie

#### 2. WeiboSuperCommentScrapy (高级评论爬虫)

```python
python "无 GUI 功能独立版/WeiboSuperCommentScrapy(no longer maintained).py"
```

**使用前需要修改：**

- 第 393 行：`username = "xxx"` 替换为微博用户名
- 第 394 行：`password = "yyy"` 替换为微博密码
- 第 397 行：`mid = 'Ha2zIe2TI'` 替换为要爬取的微博 mid

### 退出虚拟环境

```cmd
deactivate
```

## 依赖包说明

- **requests**: HTTP 请求库
- **lxml**: HTML/XML 解析库
- **Pillow**: 图像处理库（用于验证码显示）
- **rsa**: RSA 加密库（用于密码加密）
- **PyExecJS**: JavaScript 执行库（用于微博 ID 转换）

## 注意事项

1. 请确保已登录微博并获取有效的 cookie
2. 爬取频率不要过高，避免被限制
3. 建议在测试环境中先运行
4. 遵守微博的使用条款和相关法律法规

## 快速开始

1. 双击 `setup_env.bat` 创建虚拟环境
2. 双击 `activate_env.bat` 启动虚拟环境
3. 修改爬虫脚本中的配置信息
4. 运行爬虫脚本
5. 输入 `deactivate` 退出虚拟环境
