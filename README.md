# IEYI China - Beijing 2026 Official Website

International Exhibition for Young Inventors (IEYI) China 国际青少年创新发明展中国区官方网站

## 项目简介

这是 IEYI China - Beijing 2026 的官方活动网站，用于宣传和推广即将于2026年2月26-27日在北京国家速滑馆举办的国际青少年创新发明展览。

IEYI 由日本发明与创新学会（JIII）于1904年创立，旨在鼓励全球青少年的创造力和发明精神。我们的使命是：**"激励青少年创造、合作和贡献"**。

## 活动信息

- **日期**：2026年2月26-27日
- **地点**：国家速滑馆（"冰丝带"），北京
- **主题**："Inspiring Teenagers to Create, Cooperate and Contribute"
- **报名链接**：https://shuju.itccc.org.cn/f/I77TUK

## 参赛类别

### 1. 发明类（Inventions）
涵盖以下领域：
- 灾害管理（Disaster Management）
- 教育与娱乐（Education & Recreation）
- 食品与农业（Foods & Agriculture）
- 绿色技术（Green Technology）
- 安全与健康（Safety & Health）
- 特殊需求技术（Technology for Special Needs）

### 2. 艺术作品类（Artwork）
- 主题：科学的未来（Future of Science）
- 要求：原创绘画或绘画作品，不接受数字艺术
- 重点：通过视觉艺术表达科学想象力

## 技术栈

- **HTML5** - 语义化标记
- **Tailwind CSS** - 通过CDN引入，实现现代化响应式设计
- **Google Fonts** - Inter字体
- **纯静态网站** - 无需后端服务器
- **Vercel** - 部署平台

## 网站功能

### 已完成功能

1. **响应式导航栏**
   - 桌面端完整导航菜单
   - 移动端汉堡菜单
   - 平滑滚动锚点跳转
   - 固定导航条（sticky header）

2. **Hero区域**
   - 全屏背景图片
   - 渐变遮罩效果
   - 活动日期、标题、口号展示
   - 地点信息
   - 报名CTA按钮
   - 滚动指示动画

3. **关于IEYI**
   - 组织历史介绍
   - 三大核心价值展示：Create, Cooperate, Contribute
   - 图标化卡片设计

4. **参赛类别详情**
   - 发明类六大领域列表
   - 艺术作品类要求说明
   - 分类报名链接

5. **活动日程**
   - 时间轴设计
   - 2天活动安排展示
   - 响应式布局

6. **联系方式**
   - 活动协调人信息
   - 电子邮件
   - 活动地点
   - 报名入口

7. **页脚**
   - 版权信息
   - 相关链接

### 设计特点

- **现代化UI**：采用玻璃态（Glass Morphism）效果
- **渐变配色**：蓝色系主题，符合科技感定位
- **动画效果**：悬停交互、平滑过渡
- **可访问性**：语义化HTML，良好的对比度
- **移动优先**：完全响应式设计

## 项目结构

```
IEYI_website/
├── index.html              # 主页面（单页应用）
├── vercel.json            # Vercel部署配置
├── .gitignore             # Git忽略文件
├── DEPLOYMENT_GUIDE.md    # 完整部署指南
├── README.md              # 项目说明文档（本文件）
└── .vercel/               # Vercel部署缓存（已忽略）
```

## 部署信息

### 当前状态
- ✅ Git仓库已初始化
- ✅ 代码已推送至GitHub
- ✅ Vercel配置已完成
- ✅ 部署文档已编写

### 部署流程

详细部署步骤请参考 [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md)

快速部署（3步）：
```bash
# 1. 推送代码到GitHub
git push origin main

# 2. 部署到Vercel
vercel --prod

# 3. 配置自定义域名
vercel domains add your-domain.com
```

### 访问地址
- **生产环境**：待配置自定义域名
- **Vercel预览**：通过Vercel自动生成

## 开发指南

### 本地开发

1. 克隆仓库：
```bash
git clone https://github.com/your-username/IEYI_website.git
cd IEYI_website
```

2. 使用本地服务器预览：
```bash
# 使用Python
python -m http.server 8000

# 或使用Node.js
npx serve

# 或使用Vercel CLI
vercel dev
```

3. 在浏览器访问：`http://localhost:8000`

### 修改内容

由于采用纯静态HTML，直接编辑 `index.html` 即可：

- **修改文本内容**：搜索对应文字直接修改
- **修改样式**：编辑 `<style>` 标签内的CSS或Tailwind类名
- **修改链接**：更新 `href` 属性
- **修改图片**：替换背景图URL（第15行）

### 提交更新

```bash
git add .
git commit -m "描述你的更改"
git push origin main

# 推送后Vercel会自动重新部署
```

## 联系方式

**活动协调人**：Chen Zhengrong (Fred)
**邮箱**：chenzhengrong@itccc.org
**活动地点**：国家速滑馆，北京

## 版权信息

© 2026 IEYI China. All rights reserved.

## Git提交历史

最近更新：
- `90fcb37` - feat: update artwork requirements and schedule
- `024cd2f` - chore: remove temporary documentation files
- `de47f54` - docs: rewrite deployment guide with comprehensive instructions
- `637ecb6` - chore: remove CNAME file
- `bae896c` - feat: add CNAME for custom domain

## 相关资源

- [IEYI官方网站](https://www.ieyi.org/)
- [Vercel官方文档](https://vercel.com/docs)
- [Tailwind CSS文档](https://tailwindcss.com/docs)
- [项目部署指南](./DEPLOYMENT_GUIDE.md)
