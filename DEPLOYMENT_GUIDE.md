# Vercel + GitHub 网站部署完整指南

> 适用于静态网站、单页应用、跳转页等所有前端项目的快速部署

## 📋 前置要求（仅首次需要）

```bash
# 检查工具是否已安装
which gh        # GitHub CLI
which vercel    # Vercel CLI

# 如未安装，使用 Homebrew 安装：
brew install gh vercel

# 首次使用需要登录
gh auth login       # GitHub 认证
vercel login        # Vercel 认证
```

## 🚀 标准部署流程（3步完成）

### 步骤 1：推送代码到 GitHub

```bash
cd /path/to/your/project

# 如果还没有 Git 仓库
git init
git add .
git commit -m "feat: initial commit"

# 创建 GitHub 仓库并推送
gh repo create 项目名 --public --source=. --remote=origin
git push -u origin main
```

### 步骤 2：部署到 Vercel

```bash
# 部署到生产环境
vercel --prod
```

### 步骤 3：配置自定义域名

```bash
# 添加自定义域名（例如：subdomain.itccc.app）
vercel domains add subdomain.itccc.app

# 如果域名在 Vercel 管理，会自动配置
# 如果域名在其他服务商（如 Cloudflare），需要手动配置 DNS
```

**完成！** 🎉 访问你的网站：`https://subdomain.itccc.app`

---

## 📁 项目文件配置

### 静态网站项目结构

```
your-project/
├── index.html          # 主页面（必需）
├── vercel.json         # Vercel 配置（推荐）
└── .gitignore          # Git 忽略文件
```

### 示例：跳转页面
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="refresh" content="0;url=目标网址">
    <title>跳转中...</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
        }
        .container {
            text-align: center;
            padding: 40px;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
        }
        .spinner {
            width: 50px;
            height: 50px;
            margin: 0 auto 20px;
            border: 4px solid #f3f3f3;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        h1 { color: #333; font-size: 24px; margin: 0 0 10px 0; }
        p { color: #666; font-size: 14px; margin: 0 0 20px 0; }
        a { color: #667eea; text-decoration: none; font-weight: 500; }
        a:hover { text-decoration: underline; }
    </style>
</head>
<body>
    <div class="container">
        <div class="spinner"></div>
        <h1>正在跳转...</h1>
        <p>如果没有自动跳转，请点击下方链接</p>
        <a href="目标网址">点击这里访问</a>
    </div>
    <script>
        window.location.href = '目标网址';
    </script>
</body>
</html>
```

### vercel.json 配置文件

```json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

### .gitignore 文件

```
.vercel
node_modules
.DS_Store
```

---

## 🔧 域名 DNS 配置详解

### 场景 1：域名在 Vercel 注册 ✅ 推荐

**特点：** 完全自动化，零配置

```bash
# 添加域名后，Vercel 自动完成所有 DNS 配置
vercel domains add subdomain.itccc.app
```

等待 1-5 分钟，域名即可访问。无需任何手动操作！

### 场景 2：域名 Nameservers 在 Cloudflare ⚠️

**问题：** 即使域名在 Vercel 注册，如果 Nameservers 指向 Cloudflare，需要手动配置

**检查 Nameservers：**
```bash
dig NS itccc.app +short
```

**如果输出显示 `cloudflare.com`：**
```
ivy.ns.cloudflare.com
zeus.ns.cloudflare.com
```

**解决方案（3选1）：**

#### 方案 A：切换回 Vercel Nameservers（推荐）

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 找到域名 `itccc.app`
3. 删除该域名（或移除该域名托管）
4. 等待 5-10 分钟，Vercel 会自动接管 nameservers

验证 nameservers 已切换：
```bash
dig NS itccc.app +short
# 应该看到：
# ns1.vercel-dns.com
# ns2.vercel-dns.com
```

#### 方案 B：在 Cloudflare 添加 DNS 记录

登录 Cloudflare → 域名管理 → DNS → 添加记录：

```
类型: A
名称: subdomain
IPv4 地址: 76.76.21.21
代理状态: 已代理（橙色云朵）
TTL: 自动
```

#### 方案 C：使用 CNAME 记录（备选）

```
类型: CNAME
名称: subdomain
目标: cname.vercel-dns.com
代理状态: 已代理（橙色云朵）
TTL: 自动
```

### 场景 3：域名在其他注册商

在域名注册商处添加 DNS 记录：

```
类型: A
主机记录: subdomain
记录值: 76.76.21.21
```

或使用 CNAME：
```
类型: CNAME
主机记录: subdomain
记录值: cname.vercel-dns.com
```

---

## ✅ 验证部署成功

### 1. 检查 DNS 解析

```bash
# 检查域名解析
dig subdomain.itccc.app +short

# 应该看到 Vercel 的 IP 地址（通常是 76.76.21.21 或 Cloudflare 的 IP）
```

### 2. 检查网站访问

```bash
# 检查 HTTP 状态码
curl -I https://subdomain.itccc.app

# 应该看到 HTTP/2 200
```

### 3. 浏览器访问

直接在浏览器中打开：`https://subdomain.itccc.app`

**如果浏览器看不到，但 curl 能访问：**
- 清除浏览器缓存：`Cmd+Shift+R` (Mac) 或 `Ctrl+Shift+R` (Windows)
- 使用隐私/无痕模式访问
- 等待 DNS 缓存刷新（可能需要几分钟）

---

## 🔧 常见问题排查

### 问题 1：域名显示 HTTP 421 错误

**症状：**
```bash
curl -I https://subdomain.itccc.app
# HTTP/2 421 Misdirected Request
```

**原因：** Nameservers 配置问题，域名未正确关联到 Vercel

**解决方案：**
```bash
# 1. 检查 nameservers
dig NS itccc.app +short

# 2. 如果显示 Cloudflare nameservers，切换回 Vercel
# 参考上方"场景 2"的解决方案

# 3. 如果已经是 Vercel nameservers，删除并重新添加域名
vercel domains rm subdomain.itccc.app
vercel domains add subdomain.itccc.app

# 4. 重新部署
vercel --prod
```

### 问题 2：DNS 解析正确但无法访问

**症状：**
```bash
dig subdomain.itccc.app +short
# 76.76.21.21  ← 有 IP 返回

curl -I https://subdomain.itccc.app
# 超时或连接失败
```

**解决方案：**
1. 等待 SSL 证书生成（可能需要 5-10 分钟）
2. 检查 Vercel 部署状态：`vercel ls`
3. 查看域名详情：`vercel domains inspect subdomain.itccc.app`

### 问题 3：Nameservers 切换后仍显示旧配置

**原因：** DNS 缓存

**解决方案：**
```bash
# macOS 清除 DNS 缓存
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder

# 等待 5-10 分钟让全球 DNS 更新
# 使用其他 DNS 服务器查询
dig @8.8.8.8 subdomain.itccc.app
```

### 问题 4：浏览器能访问，curl 不能（或相反）

**原因：** 浏览器缓存或 DNS 缓存不一致

**解决方案：**
- 浏览器：硬刷新 `Cmd+Shift+R` 或使用无痕模式
- 命令行：清除 DNS 缓存（见上方）
- 等待 DNS 传播完成（最多 24 小时，通常 1 小时内）

### 问题 5：更新代码后网站未更新

**解决方案：**
```bash
# 1. 确认代码已推送到 GitHub
git push origin main

# 2. 手动触发 Vercel 重新部署
vercel --prod

# 3. 清除浏览器缓存
```

### 问题 6：`vercel --prod` 报错 `Error: Invalid JSON response`

**症状：**
```
Deploying fredchans-projects/ieyi-website
Error: Invalid JSON response
```

**原因：** 系统环境变量 `NODE_TLS_REJECT_UNAUTHORIZED=0` 与 Vercel API 的 TLS 握手存在冲突，导致上传请求返回非 JSON 内容。

**解决方案：**
```bash
# 部署时临时覆盖该环境变量，强制启用 TLS 证书验证
NODE_TLS_REJECT_UNAUTHORIZED=1 vercel --prod --yes
```

> **注意：** 后续每次手动部署都需要加 `NODE_TLS_REJECT_UNAUTHORIZED=1` 前缀，或在 shell 配置文件（`~/.zshrc`）中彻底移除 `NODE_TLS_REJECT_UNAUTHORIZED=0` 的设置。

### 问题 7：上传体积过大导致 `Unexpected end of JSON input`

**症状：**
```
Uploading [====================] (1.5MB/1.5MB)
Error: FetchError: invalid json response body at https://api.vercel.com/v2/files
  reason: Unexpected end of JSON input
```

**原因：** 项目目录中存在 PDF 等大文件（本项目约 1.5MB），上传时服务端响应被截断。

**解决方案：** 在项目根目录创建 `.vercelignore`，排除不需要部署的大文件：

```
# .vercelignore
*.pdf
DEPLOYMENT_GUIDE.md
README.md
```

添加后上传体积从 1.5MB 降至几十 KB，部署立即成功。

> **最佳实践：** 所有新项目都应提前创建 `.vercelignore`，排除文档、设计稿、原始素材等非网站资源。

---

## 💡 最佳实践

### 1. 域名管理统一化

**推荐：** 所有域名使用 Vercel Nameservers
- ✅ 零配置，自动化
- ✅ SSL 证书自动续期
- ✅ 全球 CDN 加速
- ❌ 避免使用 Cloudflare 作为 DNS 代理（除非有特殊需求）

### 2. 项目配置标准化

每个项目都应包含：
- `vercel.json` - 路由和重定向配置
- `.vercelignore` - 排除 PDF、文档等非网站资源，避免上传体积过大
- `.gitignore` - 忽略 `.vercel` 目录
- `README.md` - 项目说明文档

### 3. 部署自动化

设置 GitHub Actions 实现自动部署：

```yaml
# .github/workflows/deploy.yml
name: Deploy to Vercel
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
```

### 4. 环境管理

- **生产环境：** `vercel --prod`
- **预览环境：** `vercel` （每个 Git 分支自动创建预览）
- **本地测试：** `vercel dev`

---

## 🎓 总结

### 核心要点

1. **3 步完成部署**：GitHub → Vercel → 域名配置
2. **Nameservers 是关键**：确保使用 Vercel nameservers
3. **DNS 需要时间**：最多等待 24 小时（通常 5-10 分钟）
4. **验证很重要**：使用 `dig` 和 `curl` 验证配置

### 时间预期

| 操作 | 耗时 |
|------|------|
| 代码推送 GitHub | 30 秒 |
| Vercel 部署 | 1-2 分钟 |
| 添加域名 | 10 秒 |
| DNS 生效 | 1-10 分钟 |
| **总计** | **5-15 分钟** |

### 快速检查清单

- [ ] 代码已推送到 GitHub
- [ ] Vercel 部署成功（绿色勾选）
- [ ] 域名已添加到项目
- [ ] Nameservers 指向 Vercel
- [ ] DNS 解析返回正确 IP
- [ ] HTTPS 访问返回 200 状态码
- [ ] 浏览器能正常访问

---

## 📚 相关资源

- [Vercel 官方文档](https://vercel.com/docs)
- [GitHub CLI 文档](https://cli.github.com/)
- [DNS 知识库](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [本项目示例](https://github.com/Fred-Chan/IEYI_website)
