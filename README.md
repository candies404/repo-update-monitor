# 仓库更新检查工作流

一个用于自动检查 GitHub 仓库更新并发送通知的 GitHub Actions 工作流。

## 功能特点

- 🔄 自动检查多个仓库的更新状态
- ⏰ 支持定时和手动触发
- 📊 生成详细的更新报告
- 📱 支持多渠道通知（微信、钉钉）
- 🌏 使用北京时间

## 快速开始

### 1. 配置仓库

在你的仓库中创建以下文件：
- `.github/workflows/check-updates.yml`

### 2. 设置环境变量

在 Settings > Secrets and variables > Actions > Variables 中添加：

- `REPOS`：要监控的仓库列表
  ```
  vastsa/FileCodeBox MartialBE/one-hub
  ```
  注意：使用空格分隔多个仓库

### 3. 设置通知渠道

在 Settings > Secrets and variables > Actions > Secrets 中添加：

- 微信推送（可选）
   - `wpush_key`：WxPusher 的 API key

- 钉钉机器人（可选）
   - `dd_bot_secret`：加签密钥
   - `dd_bot_token`：访问令牌

## 工作流说明

### 触发条件

- ⏱️ 定时触发：每天北京时间 8:00（UTC 0:00）
- 🖱️ 手动触发：支持在 Actions 页面手动运行

### 通知内容

```markdown
### 📊 统计信息

- 总仓库数：2
- 有更新：1
- 无更新：1

---

### 🔍 仓库更新检查报告

### 📅 检查时间: 2024-01-01 08:00:00

---

##### ✅ 仓库：owner/repo

📝 **描述**：仓库描述
⏰ **最后更新**：2024-01-01 07:30:00
📌 **最新提交**：更新说明
⏳ **状态**：0.5小时未更新

---
```

### 更新判断

- ✅ 24小时内有更新
- ❌ 24小时内无更新

## 自定义配置

### 修改检查频率

编辑 `.github/workflows/check-updates.yml` 中的 cron 表达式：

```yaml
on:
  schedule:
    - cron: '0 0 * * *'  # 默认每天 UTC 0:00
```

### 通知选项

在通知步骤中配置：

```yaml
- name: 发送通知
  uses: candies404/Multi-Channel-Notifier@latest
  with:
    hitokoto: 'false'  # 是否显示一言
    dd_msg_type: 'markdown'  # 钉钉消息格式
```

## 依赖项目

- [Multi-Channel-Notifier](https://github.com/candies404/Multi-Channel-Notifier)

## 注意事项

1. 确保仓库列表格式正确（使用空格分隔）
2. 至少配置一个通知渠道
3. 需要有访问仓库的权限
4. API 请求可能受到 GitHub 限制

## 常见问题

Q: 如何添加新的仓库监控？  
A: 在 GitHub Variables 中编辑 `REPOS` 变量，添加新的仓库（用空格分隔）

Q: 为什么收不到通知？  
A: 检查通知渠道的配置是否正确，密钥是否正确设置

Q: 如何修改检查的时间间隔？  
A: 修改 cron 表达式，例如 `0 */12 * * *` 表示每12小时检查一次
