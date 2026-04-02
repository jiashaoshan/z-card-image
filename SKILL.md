---
name: z-card-image
version: 1.2.0
description: 小红书内容卡片渲染引擎 - 将文案渲染成 PNG 卡片图。支持封面图、长文分页卡片、文字海报。3:4竖版比例（900×1200），白色背景+红色高亮配色，2.5倍行距。
metadata:
  openclaw:
    requires:
      bins:
        - python3
        - google-chrome
---

# z-card-image

小红书内容卡片渲染引擎，将文案渲染成精美 PNG 卡片图。

## 核心功能

- ✅ **封面渲染** - 红色粗黑标题 + 副标题 + 字数/阅读时间 footer
- ✅ **内容卡片** - 长文分页，2.5倍行距，阅读舒适
- ✅ **3:4 竖版** - 900×1200 标准比例，适配小红书
- ✅ **高亮配色** - 白色背景 #ffffff + 正红色 #E60012
- ✅ **Markdown支持** - 支持粗体、列表、标题等格式

## 环境要求

- Python 3
- Google Chrome（macOS：`/Applications/Google Chrome.app`；Linux：`chromium`）

## 使用方法

### 1. 渲染封面

```bash
python3 scripts/render_article.py \
  --title "带父母旅游的目的地推荐" \
  --text "4维评估模型+12个实测目的地" \
  --page-num 1 \
  --page-total 1 \
  --out output/cover.png \
  --footer "字数 3500 | 阅读约 10 分钟" \
  --bg "#ffffff" \
  --highlight "#E60012" \
  --cover
```

### 2. 渲染内容卡片

```bash
python3 scripts/render_article.py \
  --title "文章标题" \
  --text "该页正文内容..." \
  --page-num 1 \
  --page-total 8 \
  --out output/card_01.png \
  --footer "第 1/8 页" \
  --bg "#ffffff" \
  --highlight "#E60012"
```

### 3. 批量渲染长文

```bash
# 方式1：LLM分页后逐页调用（推荐）
# 每页单独调用，精确控制分页

# 方式2：批量模式（兼容保留）
python3 scripts/render_article.py \
  --title "文章标题" \
  --text "全文内容..." \
  --out-dir output/cards/ \
  --chars-per-page 280
```

## 参数说明

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--title` | 文章标题 | 必填 |
| `--text` | 正文内容 | 必填 |
| `--page-num` | 当前页码 | 1 |
| `--page-total` | 总页数 | 1 |
| `--out` | 输出文件路径 | 必填 |
| `--footer` | 底部文字 | `字数 XXX | 阅读约 X 分钟` |
| `--bg` | 背景色（hex） | `#ffffff` |
| `--highlight` | 高亮色（hex） | `#E60012` |
| `--cover` | 封面模式标志 | - |
| `--chars-per-page` | 每页字符数（批量模式） | 280 |

## 配色预设

### 小红书标准配色（默认）

```bash
--bg "#ffffff"           # 白色背景
--highlight "#E60012"    # 正红色高亮
```

### 其他平台

| 平台 | `--bg` | `--highlight` |
|------|--------|--------------|
| 小红书 | `#ffffff` | `#E60012` |
| 公众号 | `#e6f5ef` | `#22a854` |
| 微博 | `#fff8e6` | `#ff8200` |

## 模板索引

| 模板名 | 比例 | 尺寸 | 用途 |
|--------|------|------|------|
| `cover-3-4` | 3:4 | 900×1200 | 封面图 |
| `article-3-4` | 3:4 | 900×1200 | 内容卡片 |
| `poster-3-4` | 3:4 | 900×1200 | 文字海报 |
| `wechat-cover-split` | 335:100 | 1340×400 | 公众号封面 |

## 文件结构

```
z-card-image/
├── scripts/
│   ├── render_article.py      # 文章卡片渲染（主脚本）
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
  font-size: 110px;           /* 主标题字号 */
  color: {{HIGHLIGHT_COLOR}}; /* 标题颜色 */
  font-weight: 900;           /* 超粗 */
  line-height: 1.5;           /* 行距 */
  margin-top: 200px;          /* 顶部距离 */
}
```

### 修改内容卡片样式

编辑 `assets/templates/article-3-4.html`：

```css
.content {
  font-size: 28px;      /* 正文字号 */
  line-height: 2.5;     /* 行距 */
  color: #2a2a2a;       /* 正文颜色 */
}
```

## 输入校验

- **文案超出模板字数上限**：自动拆分后再渲染
- **封面标题过长**：建议压缩成2-3行短标题
- **正文超长**：LLM负责分页，每页约280字符

## 新增模板

1. 新建 `assets/templates/<name>.html`
2. 在 `scripts/render_card.py` 的 `size_map` 里注册尺寸
3. 在上方模板索引中添加一行
4. 创建 `references/<name>.md` 记录参数规范

## 调用示例

### xiaohongshu-auto-m2img 调用方式

```python
cmd = [
    'python3', 'z-card-image/scripts/render_article.py',
    '--title', '标题',
    '--text', content,
    '--page-num', str(i),
    '--page-total', str(total),
    '--out', output_file,
    '--footer', f'第 {i}/{total} 页',
    '--bg', '#ffffff',
    '--highlight', '#E60012'
]
subprocess.run(cmd)
```

## 更新日志

### V1.2.0 (2026-04-02)
- ✅ 独立仓库管理
- ✅ 小红书标准配色（白色背景+正红色）
- ✅ 封面模板优化（标题行距1.5）
- ✅ 内容卡片行距2.5，阅读舒适
- ✅ 简化参数，专注小红书场景

### V1.1.0
- 支持封面渲染
- 支持长文分页
- Markdown格式支持
