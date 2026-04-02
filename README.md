# z-card-image

**小红书内容卡片渲染引擎** - 将文案渲染成精美 PNG 卡片图。

## 特性

- 📸 **3:4 竖版** - 900×1200 标准比例，完美适配小红书
- 🎨 **小红书配色** - 白色背景 + 正红色高亮，清爽专业
- 📖 **2.5倍行距** - 阅读舒适不拥挤
- ✨ **封面渲染** - 红色粗黑标题 + 字数统计
- 📄 **长文分页** - 自动分页，每页约280字符
- 💪 **Markdown支持** - 粗体、列表、标题自动渲染

## 快速开始

### 环境要求

- Python 3
- Google Chrome

### 渲染封面

```bash
python3 scripts/render_article.py \
  --title "带父母旅游的目的地推荐" \
  --text "4维评估模型+12个实测目的地" \
  --out cover.png \
  --footer "字数 3500 | 阅读约 10 分钟" \
  --cover
```

### 渲染内容卡片

```bash
python3 scripts/render_article.py \
  --title "文章标题" \
  --text "正文内容..." \
  --page-num 1 \
  --page-total 8 \
  --out card_01.png
```

## 参数说明

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--title` | 文章标题 | 必填 |
| `--text` | 正文内容 | 必填 |
| `--out` | 输出文件路径 | 必填 |
| `--footer` | 底部文字 | `字数 XXX \| 阅读约 X 分钟` |
| `--bg` | 背景色 | `#ffffff` |
| `--highlight` | 高亮色 | `#E60012` |
| `--cover` | 封面模式 | - |

## 配色方案

### 小红书标准配色（默认）

```
背景色: #ffffff (纯白)
高亮色: #E60012 (正红)
```

### 其他平台

```bash
# 公众号风格
--bg "#e6f5ef" --highlight "#22a854"

# 微博风格
--bg "#fff8e6" --highlight "#ff8200"
```

## 文件结构

```
z-card-image/
├── scripts/
│   ├── render_article.py      # 文章渲染主脚本
│   ├── render_card.py         # 单张卡片渲染
│   └── render_x_like_posts.py # X风格帖子渲染
├── assets/
│   ├── templates/
│   │   ├── cover-3-4.html     # 封面模板
│   │   └── article-3-4.html   # 内容卡片模板
│   ├── styles/
│   │   └── md.css             # Markdown样式
│   └── icons/                 # 图标资源
└── references/                # 模板规范文档
```

## 样式定制

### 修改封面样式

编辑 `assets/templates/cover-3-4.html`：

```css
.title { 
  font-size: 110px;      /* 主标题字号 */
  font-weight: 900;      /* 超粗 */
  line-height: 1.5;      /* 行距 */
}
```

### 修改内容卡片样式

编辑 `assets/templates/article-3-4.html`：

```css
.content {
  font-size: 28px;    /* 正文字号 */
  line-height: 2.5;   /* 行距 */
}
```

## 使用场景

- ✅ 小红书图文笔记
- ✅ 公众号文章封面
- ✅ 金句海报
- ✅ 知识卡片
- ✅ 读书笔记分享

## 调用示例

```python
import subprocess

# 渲染封面
subprocess.run([
    'python3', 'z-card-image/scripts/render_article.py',
    '--title', '标题',
    '--text', '副标题',
    '--out', 'cover.png',
    '--footer', '字数 1000 | 阅读约 5 分钟',
    '--cover'
])

# 渲染内容卡片
subprocess.run([
    'python3', 'z-card-image/scripts/render_article.py',
    '--title', '标题',
    '--text', '正文内容...',
    '--page-num', '1',
    '--page-total', '5',
    '--out', 'card_01.png'
])
```

## 依赖项目

- [xiaohongshu-auto-m2img](https://github.com/jiashaoshan/xiaohongshu-aicontext-md2img) - 小红书图文自动生成

## License

MIT
