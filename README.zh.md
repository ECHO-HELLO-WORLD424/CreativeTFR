# CreativeTFR

TFR 游戏的社区 UI 重制工具。基于 Vue3 构建的无服务端纯前端应用，使用 IndexedDB 进行本地存储。

[English Documentation](./README.md)

## 特性

- 基于 Vue3 的现代前端应用
- 无服务端架构 - 完全在浏览器中运行
- 使用 IndexedDB 进行本地数据持久化
- 文本输入框支持 HTML 格式
- 游戏素材图片管理系统

## 系统要求

- Node.js (v16 或更高版本) 或 Bun
- npm、yarn 或 bun 包管理器

## 安装步骤

### 1. 克隆仓库

```bash
git clone <仓库地址>
cd CreativeTFR
```

### 2. 安装依赖

```bash
npm install
```

### 3. 下载游戏素材

下载 TFR 素材数据库：[TFRdata.zip](http://997779.xyz/share/TFRdata.zip)（解压后 1.5GB）

### 4. 解压素材文件

ZIP 文件包含嵌套目录结构，需要解压并修正路径：

```bash
# 先解压到临时位置
unzip TFRdata.zip

# 移动文件到开发环境的正确位置
# 注意：ZIP 文件解压后的结构是 root/CreativeTFR/data/*
cp -r root/CreativeTFR/data/* public/data/
rm -rf root

# 验证目录结构是否正确
ls public/data/
# 应该显示：index.json, ideology/, event/, flag/, focus/, leader/, news/ 等

# 同时复制到 dist 目录用于生产环境（如果已经构建过）
cp -r public/data dist/
```

**一键执行方式：**

```bash
unzip TFRdata.zip && cp -r root/CreativeTFR/data/* public/data/ && rm -rf root && cp -r public/data dist/
```

### 5. 设置国家精神图片

国家精神管理器需要将图片放置在 `public/data/spirit/` 目录。复制预设图片并重新生成索引：

```bash
# 创建 spirit 目录
mkdir -p public/data/spirit

# 从 preset 复制国家精神图片
cp public/preset/*.png public/data/spirit/

# 重新生成 index.json 以包含国家精神图片
cd public/data
python3 ftojson.py
cd ../..
```

这样可以确保国家精神管理器对话框中正确显示内置图片。

## 开发环境

启动开发服务器：

```bash
npm run dev
```

应用将在 `http://localhost:5173` 运行（如果 5173 端口被占用则使用其他端口）。

## 生产环境构建

### 构建应用

```bash
npm run build
```

这将在 `dist/` 目录中创建优化后的生产环境构建。

### 本地预览生产构建

```bash
npm run preview
```

### 部署到生产环境

将 `dist/` 文件夹部署到任何静态文件托管服务：

- **Nginx**：将文档根目录指向 `dist/` 目录
- **Apache**：配置虚拟主机以提供 `dist/` 目录
- **静态托管**：将 `dist/` 上传到 Netlify、Vercel、GitHub Pages 等

#### Nginx 配置示例

```nginx
server {
    listen 80;
    server_name yourdomain.com;
    root /path/to/CreativeTFR/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

## 重要说明

- **数据存储**：所有用户数据存储在浏览器的 IndexedDB 中。不支持云同步或多设备同步。
- **HTML 支持**：输入框支持原始 HTML 格式。
- **素材结构**：游戏素材必须放在 `public/data/` 目录（开发环境），构建时会自动复制到 `dist/data/`。

## 自定义图片要求

应用支持上传自定义图片用于各种元素。为获得最佳效果，请使用以下尺寸：

| 图片类型 | 推荐尺寸 | 格式 | 说明 |
|---------|---------|------|------|
| **意识形态 (Ideology)** | 66 x 68 像素 | PNG（支持透明） | 政治意识形态图标 |
| **领导人 (Leader)** | 156 x 210 像素 | PNG | 人物肖像 |
| **国旗 (Flag)** | 82 x 52 像素 | PNG（支持透明） | 国旗图标 |
| **阵营 (Faction)** | 60 x 60 像素 | PNG（支持透明） | 联盟/阵营图标 |
| **国策 (Focus)** | 90 x 93 像素 | PNG（支持透明） | 国策树图标 |
| **事件 (Event)** | 500 x 250 像素 | PNG | 事件图片（2:1 比例） |
| **新闻 (News)** | 400 x 150 像素 | PNG | 新闻文章配图 |
| **超级事件 (Super Event)** | 982 x 594 像素 | PNG | 大型事件图片 |

**自定义图片提示：**
- 使用 PNG 格式以支持透明度
- 匹配推荐尺寸以获得最佳视觉质量
- 某些类型（意识形态、阵营、国策、事件）的图片可在编辑器中调整大小
- 保持合理的文件大小以确保浏览器性能

## 项目结构

```
CreativeTFR/
├── public/           # 静态资源（在根 URL 提供）
│   └── data/        # 游戏素材（图片、index.json）
├── src/             # Vue3 源代码
├── dist/            # 生产环境构建输出
└── package.json     # 项目依赖
```

## 故障排查

**开发环境中图片无法加载？**
- 确保素材已解压到 `public/data/`
- 检查 `public/data/index.json` 是否存在
- 添加素材后重启开发服务器

**国家精神管理器中图片无法显示？**
- 运行设置命令：`mkdir -p public/data/spirit && cp public/preset/*.png public/data/spirit/`
- 重新生成索引：`cd public/data && python3 ftojson.py && cd ../..`
- 检查 `public/data/spirit/` 目录是否包含 PNG 文件

**生产环境中图片无法加载？**
- 验证构建后 `dist/data/` 目录是否存在
- 检查 Web 服务器配置是否正确提供静态文件
- 确保构建过程成功完成

## 许可证

TFR 游戏社区项目。
