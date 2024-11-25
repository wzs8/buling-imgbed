<div align="center">
    <h1>🌈 布灵图床</h1>
    <p>一个基于 Cloudflare 全家桶的个人图床解决方案</p>
    <p>
        <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="license">
        <img src="https://img.shields.io/badge/version-1.0.0-brightgreen.svg" alt="version">
        <img src="https://img.shields.io/badge/Hono-3.x-E36002.svg" alt="hono">
        <img src="https://img.shields.io/badge/Cloudflare-Workers-F38020.svg" alt="cloudflare">
        <img src="https://img.shields.io/badge/Cloudflare%20R2-Storage-F38020.svg" alt="r2">
        <img src="https://img.shields.io/badge/Cloudflare%20D1-Database-F38020.svg" alt="d1">
        <img src="https://img.shields.io/badge/Telegram-Bot-26A5E4.svg" alt="telegram">
    </p>
    <p>
        <a href="https://img.808080.xyz">🌍 在线演示</a> ·
        <a href="#部署指南">🚀 部署指南</a> ·
        <a href="#使用文档">📚 使用文档</a> ·
        <a href="https://anuuu.com/buling-imgbed.html">🎯 项目详解</a>
    </p>
</div>

> 🎯 基于 Cloudflare 的现代化图床解决方案，零成本易部署，为个人提供便捷的图片托管服务。
> 
> ⚠️ 注意：本项目为后端服务，部署完成后可通过 Telegram 机器人进行图片上传。
> 如需网页端上传和管理功能，请部署配套的前端项目：[布灵图床前端](https://github.com/wzs8/buling-imgbed-frontend)



## 🪄 特性一览

- **零成本部署**
  - ☁️ 完全基于 Cloudflare 免费服务
  - 🆓 每月 100k 请求完全免费
  - ⚡ 全球 CDN 加速
  
- **简单易用**
  - 🔐 绑定 Telegram 账号
  - 🤖 Telegram Bot 快速上传
  - 📋 多种图片链接格式



## 📸 效果预览

<table>
  <tr>
    <td><img src="https://s1.img.808080.xyz/07a0de2be6678c5e27e75a7e5a646cce.png" alt="首页预览"></td>
    <td><img src="https://s1.img.808080.xyz/410f2aeb7b63e9048ed55f99df050627.png" alt="上传预览"></td>
  </tr>
</table>

## 🚀 部署指南

### 部署准备

- Cloudflare 账号（生成CF_API_TOKEN、CF_ACCOUNT_ID，以及已经激活R2存储桶）
- JWT 密钥（可使用随机生成UUID）
- Telegram Bot Token（可选）

<details>
<summary>📝 如何获取这些配置？ · <a href="https://anuuu.com/buling-imgbed-config.html">📖 查看图文教程</a></summary>

#### 1. Cloudflare 配置获取
1. 注册并登录 [Cloudflare](https://dash.cloudflare.com)
2. 获取 Account ID：
   - 登录后点击右上角的账号图标
   - 在下拉菜单中选择 "Account Home"
   - 在右侧可以找到你的 Account ID
3. 创建 API Token：
   - 进入 [API Tokens 页面](https://dash.cloudflare.com/profile/api-tokens)
   - 点击 "Create Token"
   - 选择 "Create Custom Token"
   - 权限设置：
     - Account.Workers R2 Storage: Read & Write
     - Account.Workers Scripts: Edit
     - Account.D1: Edit
     - Account.Cloudflare Pages: Edit (建议添加，用于前端项目部署，可复用此token)

#### 2. JWT 密钥生成
- 方法一：使用在线 UUID 生成器：[UUID Generator](https://www.uuidgenerator.net/)
- 方法二：使用命令行：
  ```bash
  # Linux/Mac
  uuidgen
  # 或者
  python -c 'import uuid; print(uuid.uuid4())'
  ```

#### 3. Telegram Bot Token 获取
1. 在 Telegram 中找到 [@BotFather](https://t.me/BotFather)
2. 发送 `/newbot` 命令
3. 按照提示设置机器人名称
4. 创建成功后，BotFather 会发送给你 Bot Token

</details>

### GitHub一键部署（🌟推荐）
👉 [查看详细图文部署教程](https://anuuu.com/buling-imgbed-backend-deploy.html)

1. Fork 本仓库
2. 配置 GitHub Secrets:
   ```
   CF_API_TOKEN=your_cloudflare_api_token
   CF_ACCOUNT_ID=your_cloudflare_account_id
   JWT_SECRET=your_jwt_secret
   TG_BOT_TOKEN=your_telegram_bot_token
   ```
3. 存储桶区域设置（可选）：
   - 默认在亚太地区创建存储桶
   - 如需更改区域，请修改 `.github\workflows\deploy.toml` 中的以下配置：
     ```yaml
     wrangler r2 bucket create buling-imgbed-r2 --location=<region>
     ```
   - 可选区域：
     - `apac` - 亚太地区（默认）
     - `wnam` - 北美西部
     - `enam` - 北美东部
     - `weur` - 欧洲西部
     - `eeur` - 欧洲东部
4. 启用 GitHub Actions
5. 推送代码触发自动部署（首次部署请点击 Actions 页面手动触发）


### 手动部署

```bash
# 安装依赖
npm install

# 本地开发
npm run dev

# 部署到 Cloudflare
npm run deploy
```

## 环境变量
| 变量 | 是否必要 | 默认值 | 备注 |
| ---- | ----- | ----- | ---- |
| CF_ACCOUNT_ID | 是 | | |
| CF_API_TOKEN  | 是 | | |
| JWT_SECRET    | 是 | | |
| TG_BOT_TOKEN  | 否 | | |
| CF_D1_DATABASE | 否 | buling-imgbed-d1 | |
| CF_R2_BUCKET | 否 | buling-imgbed-r2 | |
| CF_LOCATION | 否 | apac | |

*说明：`CF_R2_BUCKET`:存储桶名称只能包含字母数字和 - 字符。*


## 🔧 技术架构

```
Cloudflare Workers (服务运行时)
    ├── Hono.js (Web 框架)
    ├── Cloudflare D1 (SQLite 数据库)
    ├── Cloudflare R2 (对象存储)
    └── Cloudflare CDN (全球加速)
```

## 🤝 贡献指南

欢迎提交 PR、Issue 或者加入讨论组参与开发！

提交代码前请确保：
1. 遵循现有的代码风格
2. 添加必要的测试和文档
3. commit message 遵循 [约定式提交](https://www.conventionalcommits.org/zh-hans/v1.0.0/)

## 📜 开源协议

本项目采用 [MIT](LICENSE) 协议开源。

## 🎉 鸣谢
- [Cloudflare](https://www.cloudflare.com/) - 提供优秀的基础设施服务
- [Vue](https://vuejs.org/) - 优秀的 Web 框架
- [Nuxt](https://nuxt.com/) - 优秀的 Web 框架
- [开源社区](https://github.com/) - 感谢所有开源贡献者

感谢所有为这个项目做出贡献的开发者们！

## 📞 联系作者

- 博客：[Anuuu.com](https://anuuu.com)
- Telegram：[@wzsxh]

---

<div align="center">
    如果这个项目对你有帮助，请考虑给它一个 ⭐️
</div>
