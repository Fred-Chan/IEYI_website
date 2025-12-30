# 域名跳转快速部署指南

## 📋 前置要求检查（仅首次）

```bash
# 检查工具是否已安装
which gh        # GitHub CLI
which vercel    # Vercel CLI

# 如未安装，执行：
# brew install gh vercel
```

## 🚀 标准部署流程（5步完成）

### 1. 创建项目文件

**必需文件：**

#### `index.html` - 跳转页面
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

#### `vercel.json` - Vercel配置（可选但推荐）
```json
{
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/index.html"
    }
  ]
}
```

#### `.gitignore` - Git忽略文件
```
.vercel
```

### 2. 初始化并推送到 GitHub

```bash
# 初始化Git
git init
git add .
git commit -m "feat: 初始化跳转页面"

# 创建GitHub仓库并推送（一条命令完成）
gh repo create 项目名 --public --source=. --remote=origin
git push -u origin main
```

### 3. 部署到 Vercel

```bash
# 一条命令完成部署
vercel --prod --yes
```

### 4. 添加自定义域名

```bash
# 添加域名
vercel domains add 你的域名.itccc.app
```

### 5. 配置 DNS（在域名服务商）

**如果域名在 Vercel：**
- 自动配置，无需操作

**如果域名在 Cloudflare 等其他服务商：**
- 类型：`CNAME`
- 名称：子域名部分（如 `tiaozhanying`）
- 值：`cname.vercel-dns.com`
- 代理状态：开启（橙色云朵）

---

## ⚡ 超快速模板（复制粘贴即用）

```bash
# === 设置变量（修改这里） ===
PROJECT_NAME="项目名"
CUSTOM_DOMAIN="子域名.itccc.app"
TARGET_URL="https://目标网址.com"

# === 创建index.html ===
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="refresh" content="0;url=TARGET_URL_PLACEHOLDER">
    <title>跳转中...</title>
    <style>
        body{margin:0;padding:0;display:flex;justify-content:center;align-items:center;min-height:100vh;background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif}
        .container{text-align:center;padding:40px;background:rgba(255,255,255,0.95);border-radius:20px;box-shadow:0 20px 60px rgba(0,0,0,0.3)}
        .spinner{width:50px;height:50px;margin:0 auto 20px;border:4px solid #f3f3f3;border-top:4px solid #667eea;border-radius:50%;animation:spin 1s linear infinite}
        @keyframes spin{0%{transform:rotate(0deg)}100%{transform:rotate(360deg)}}
        h1{color:#333;font-size:24px;margin:0 0 10px 0}
        p{color:#666;font-size:14px;margin:0 0 20px 0}
        a{color:#667eea;text-decoration:none;font-weight:500}
        a:hover{text-decoration:underline}
    </style>
</head>
<body>
    <div class="container">
        <div class="spinner"></div>
        <h1>正在跳转...</h1>
        <p>如果没有自动跳转，请点击下方链接</p>
        <a href="TARGET_URL_PLACEHOLDER">点击这里访问</a>
    </div>
    <script>window.location.href='TARGET_URL_PLACEHOLDER';</script>
</body>
</html>
EOF

# 替换目标URL
sed -i '' "s|TARGET_URL_PLACEHOLDER|$TARGET_URL|g" index.html

# === 创建配置文件 ===
echo '.vercel' > .gitignore
echo '{"routes":[{"src":"/(.*)","dest":"/index.html"}]}' > vercel.json

# === Git + GitHub + Vercel 一键部署 ===
git init
git add .
git commit -m "feat: 初始化跳转页面"
gh repo create $PROJECT_NAME --public --source=. --remote=origin
git push -u origin main
vercel --prod --yes
vercel domains add $CUSTOM_DOMAIN

# 完成！
echo "✅ 部署完成！"
echo "📦 GitHub: https://github.com/$(gh api user -q .login)/$PROJECT_NAME"
echo "🌐 访问: https://$CUSTOM_DOMAIN"
```

---

## 🎯 关键优化点

### ❌ 不需要的检查（浪费时间）
- ~~vercel domains inspect~~（添加域名后自动配置）
- ~~vercel domains ls~~（无关当前项目）
- ~~vercel project ls~~（无关当前项目）
- ~~dig命令检查DNS~~（DNS生效需要时间，立即检查无意义）
- ~~curl测试访问~~（Vercel部署成功即可访问）

### ✅ 必要的检查（仅在出问题时）
```bash
# 如果域名访问异常，才执行以下检查：
dig 域名 +short                    # 检查DNS解析
curl -I https://域名              # 检查HTTP状态
vercel domains inspect 域名       # 查看域名配置详情
```

---

## 📊 时间对比

| 方式 | 耗时 | 说明 |
|------|------|------|
| 原流程（含检查） | ~5分钟 | 包含多次验证和检查 |
| 优化流程 | ~2分钟 | 去除不必要的检查 |
| 一键脚本 | ~30秒 | 复制粘贴修改3个变量即可 |

---

## 🔧 故障排查

### 问题：域名无法访问
```bash
# 1. 检查DNS是否生效
dig 你的域名 +short
# 有IP返回 = DNS已生效
# 无返回 = DNS未生效，等待或检查配置

# 2. 检查Vercel域名配置
vercel domains inspect 你的域名

# 3. 检查部署状态
vercel ls
```

### 问题：跳转不工作
- 检查 `index.html` 中的目标URL是否正确
- 确认文件已提交并推送到GitHub
- 确认Vercel已重新部署

---

## 💡 最佳实践

1. **使用变量替换**：创建模板文件，用 `sed` 替换URL
2. **批量部署**：将脚本保存为 `deploy-redirect.sh`，传参使用
3. **域名管理**：建议所有域名统一在一个服务商（Vercel或Cloudflare）
4. **自动化**：考虑用GitHub Actions实现持续部署

---

## 📝 批量部署脚本示例

```bash
#!/bin/bash
# 文件名：deploy-redirect.sh
# 用法：./deploy-redirect.sh 项目名 子域名 目标URL

PROJECT=$1
DOMAIN=$2.itccc.app
URL=$3

mkdir -p $PROJECT && cd $PROJECT
# ... 后续步骤同上
```

使用：
```bash
./deploy-redirect.sh tiaozhanying tiaozhanying "https://alidocs.dingtalk.com/xxx"
```

---

## 🎓 总结

**核心原则：**
- ✅ 只做必要的步骤
- ❌ 避免过度验证
- ⚡ 信任工具的自动化能力
- 🔄 出问题再排查，不要预防性检查

**最简流程：**
1. 创建文件（1个HTML + 2个配置文件）
2. Git提交 + GitHub推送（2条命令）
3. Vercel部署 + 添加域名（2条命令）
4. 完成！

总共只需要 **5个核心命令**，整个过程 **2分钟内完成**。
