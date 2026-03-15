# Folio 2025 - 中国部署指南

为了确保在中国国内服务器上顺利部署并避免网络问题，请按照以下步骤操作。

## 1. 使用国内 NPM 镜像

本项目已添加 `.npmrc` 文件，自动配置了淘宝/阿里 NPM 镜像 (`registry.npmmirror.com`) 以及 `sharp` 等二进制包的国内下载地址。

只需正常安装依赖即可：

```bash
npm install
# 或者
cnpm install
```

## 2. 字体与静态资源优化

`sources/index.html` 文件已经过修改：
- 将 Google Fonts (`fonts.googleapis.com`) 替换为国内镜像 (`fonts.font.im`)，以提高加载速度。
- 注释掉了 Google Analytics 代码，防止因网络屏蔽导致页面加载阻塞。

## 3. 构建与部署

构建项目：

```bash
npm run build
```

构建完成后，`dist` 目录下的文件即可部署到任何静态文件服务器（如 Nginx）。

### Nginx 配置示例

```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    root /path/to/folio-2025/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # 开启 gzip 压缩
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
}
```

## 4. 常见问题

- **安装 Sharp 失败**：如果遇到 `sharp` 安装问题，请确保环境变量 `sharp_binary_host` 生效，或者尝试手动删除 `node_modules` 后重新安装。
- **3D 模型加载慢**：项目包含较大的 3D 资源。如果在国内服务器上带宽不足，建议将 `assets` 或大文件托管到国内 CDN（如七牛云、阿里云 OSS 等）。

## 5. 环境变量

如果需要自行托管后端服务（本项目主要为前端静态展示，但也包含部分 WebSocket 功能），请参考 `.env.example` 配置 `.env` 文件。

```bash
cp .env.example .env
```
