# wzy的博客

> 记录数学 · 量化 · AI · Agent · ML · DL · LLM 的学习之旅

## 博客地址

[https://wzysd.github.io](https://wzysd.github.io)

## 技术栈

- [Hugo](https://gohugo.io/) - 静态网站生成器
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod) - 主题
- [GitHub Pages](https://pages.github.com/) - 托管平台
- [GitHub Actions](https://github.com/features/actions) - 自动部署

## 本地开发

```bash
# 安装 Hugo Extended (需要 Extended 版本)
# Windows: winget install Hugo.Hugo.Extended

# 克隆仓库
git clone https://github.com/wzysd/wzysd.github.io.git
cd wzysd.github.io

# 初始化子模块（如果使用 submodule）
git submodule update --init --recursive

# 本地预览
hugo server -D

# 访问 http://localhost:1313
```

## 写新文章

```bash
# 创建新文章
hugo new posts/文章标题.md

# 编辑文章
# 打开 content/posts/文章标题.md

# 本地预览
hugo server -D

# 确认无误后提交
git add .
git commit -m "feat: 添加新文章"
git push
```

## 目录结构

```
├── content/          # 博客内容
│   ├── posts/        # 文章
│   ├── courses/      # 课程笔记
│   └── about/        # 关于页面
├── layouts/          # 自定义布局
├── static/           # 静态资源（图片等）
├── themes/           # 主题
│   └── PaperMod/
├── hugo.yaml         # Hugo 配置
└── .github/
    └── workflows/
        └── hugo.yml  # 自动部署配置
```

## 功能特性

- 数学公式支持（KaTeX）
- 代码高亮
- 全站搜索
- 亮/暗主题切换
- 归档页面
- RSS 订阅
- 响应式设计

## License

MIT
