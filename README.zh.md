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

将下载的 ZIP 文件解压到指定目录：

```bash
# 解压到 public 目录（用于开发环境）
unzip TFRdata.zip -d public/data

# 如果文件结构有嵌套，需要移动到正确位置：
# 正确的结构应该是：public/data/index.json, public/data/ideology/ 等
```

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

**生产环境中图片无法加载？**
- 验证构建后 `dist/data/` 目录是否存在
- 检查 Web 服务器配置是否正确提供静态文件
- 确保构建过程成功完成

## 许可证

TFR 游戏社区项目。
