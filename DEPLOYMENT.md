# 项目部署指南

本文档介绍如何将工具集合项目部署到服务器并配置域名 `qa.tools.com`。

## 📋 部署方式选择

本项目是纯静态HTML项目，可以使用以下方式部署：

1. **Nginx部署**（推荐）- 适合自有服务器
2. **GitHub Pages + 自定义域名** - 免费，适合个人项目
3. **Netlify/Vercel** - 免费CDN，自动HTTPS
4. **Docker部署** - 容器化部署

---

## 方式一：Nginx部署（推荐）

### 1. 准备服务器

确保服务器已安装Nginx：

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install nginx

# CentOS/RHEL
sudo yum install nginx
```

### 2. 上传项目文件

将项目文件上传到服务器，建议放在 `/var/www/qa.tools.com`：

```bash
# 创建目录
sudo mkdir -p /var/www/qa.tools.com

# 上传文件（使用scp或git）
scp -r * user@your-server:/var/www/qa.tools.com/
# 或使用git
cd /var/www/qa.tools.com
git clone your-repo-url .
```

### 3. 配置Nginx

创建Nginx配置文件：

```bash
sudo nano /etc/nginx/sites-available/qa.tools.com
```

配置文件内容：

```nginx
server {
    listen 80;
    server_name qa.tools.com;
    
    # 网站根目录
    root /var/www/qa.tools.com;
    index index.html;
    
    # 日志文件
    access_log /var/log/nginx/qa.tools.com.access.log;
    error_log /var/log/nginx/qa.tools.com.error.log;
    
    # 主页面
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # 静态资源缓存
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
    
    # 安全头
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    
    # Gzip压缩
    gzip on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_types text/plain text/css text/xml text/javascript application/x-javascript application/xml+rss application/json application/javascript;
}
```

### 4. 启用站点

```bash
# 创建软链接
sudo ln -s /etc/nginx/sites-available/qa.tools.com /etc/nginx/sites-enabled/

# 测试配置
sudo nginx -t

# 重启Nginx
sudo systemctl restart nginx
```

### 5. 配置SSL证书（HTTPS）

使用Let's Encrypt免费证书：

```bash
# 安装Certbot
sudo apt install certbot python3-certbot-nginx

# 获取证书
sudo certbot --nginx -d qa.tools.com

# 自动续期（已自动配置）
sudo certbot renew --dry-run
```

Certbot会自动更新Nginx配置，添加HTTPS支持。

### 6. 配置DNS

在域名管理后台添加A记录：

```
类型: A
主机: qa
值: 你的服务器IP地址
TTL: 600
```

等待DNS生效（通常几分钟到几小时）。

---

## 方式二：GitHub Pages + 自定义域名

### 1. 创建GitHub仓库

将项目推送到GitHub：

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/tools-collection.git
git push -u origin main
```

### 2. 启用GitHub Pages

1. 进入GitHub仓库设置
2. 找到 "Pages" 选项
3. Source选择 `main` 分支，目录选择 `/ (root)`
4. 点击 Save

### 3. 配置自定义域名

在项目根目录创建 `CNAME` 文件：

```bash
echo "qa.tools.com" > CNAME
git add CNAME
git commit -m "Add custom domain"
git push
```

### 4. 配置DNS

在域名管理后台添加CNAME记录：

```
类型: CNAME
主机: qa
值: your-username.github.io
TTL: 600
```

### 5. 在GitHub中验证域名

回到GitHub Pages设置，在Custom domain中输入 `qa.tools.com` 并保存。

**注意**：GitHub Pages会自动配置HTTPS，但需要等待DNS生效。

---

## 方式三：Netlify部署

### 1. 准备项目

确保项目已推送到Git仓库（GitHub/GitLab/Bitbucket）。

### 2. 部署到Netlify

1. 访问 [Netlify](https://www.netlify.com/)
2. 点击 "Add new site" → "Import an existing project"
3. 连接Git仓库
4. 构建设置：
   - Build command: 留空（静态项目）
   - Publish directory: `/` 或留空

### 3. 配置自定义域名

1. 进入站点设置 → Domain management
2. 点击 "Add custom domain"
3. 输入 `qa.tools.com`
4. 按照提示配置DNS：
   - 添加CNAME记录：`qa` → `your-site.netlify.app`
   - 或添加A记录：指向Netlify提供的IP地址

### 4. 启用HTTPS

Netlify会自动为自定义域名配置HTTPS证书（Let's Encrypt）。

---

## 方式四：Vercel部署

### 1. 安装Vercel CLI

```bash
npm i -g vercel
```

### 2. 部署项目

```bash
cd /path/to/project
vercel
```

按照提示完成部署。

### 3. 配置自定义域名

```bash
vercel domains add qa.tools.com
```

或通过Vercel Dashboard：
1. 进入项目设置 → Domains
2. 添加 `qa.tools.com`
3. 按照提示配置DNS

### 4. 启用HTTPS

Vercel会自动配置HTTPS证书。

---

## 方式五：Docker部署

### 1. 创建Dockerfile

在项目根目录创建 `Dockerfile`：

```dockerfile
FROM nginx:alpine

# 复制项目文件到nginx目录
COPY . /usr/share/nginx/html

# 复制nginx配置
COPY nginx.conf /etc/nginx/conf.d/default.conf

# 暴露端口
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### 2. 创建nginx.conf

创建 `nginx.conf` 文件：

```nginx
server {
    listen 80;
    server_name qa.tools.com;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

### 3. 构建和运行

```bash
# 构建镜像
docker build -t tools-collection .

# 运行容器
docker run -d -p 80:80 --name tools tools-collection

# 或使用docker-compose
```

### 4. 创建docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "80:80"
    restart: unless-stopped
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
```

运行：

```bash
docker-compose up -d
```

---

## 🔧 通用配置建议

### 1. 性能优化

- 启用Gzip压缩
- 配置静态资源缓存
- 使用CDN加速（可选）

### 2. 安全配置

- 启用HTTPS（必须）
- 配置安全响应头
- 定期更新服务器和Nginx

### 3. 监控和维护

- 配置日志监控
- 设置服务器监控告警
- 定期备份配置文件

---

## 📝 部署检查清单

- [ ] 项目文件已上传到服务器
- [ ] Nginx/Web服务器配置正确
- [ ] DNS记录已配置（A或CNAME）
- [ ] SSL证书已配置（HTTPS）
- [ ] 域名解析正常（可用 `nslookup qa.tools.com` 检查）
- [ ] 网站可以正常访问
- [ ] 所有工具页面功能正常
- [ ] 移动端访问正常

---

## 🐛 常见问题

### DNS未生效

```bash
# 检查DNS解析
nslookup qa.tools.com
dig qa.tools.com

# 清除本地DNS缓存
# macOS
sudo dscacheutil -flushcache
# Linux
sudo systemd-resolve --flush-caches
```

### SSL证书问题

- 确保域名已正确解析
- 检查防火墙是否开放80和443端口
- 验证证书是否过期：`openssl s_client -connect qa.tools.com:443`

### 404错误

- 检查Nginx配置中的 `root` 路径是否正确
- 确认 `index.html` 文件存在
- 检查文件权限：`sudo chown -R www-data:www-data /var/www/qa.tools.com`

---

## 📞 需要帮助？

如果遇到部署问题，请检查：
1. 服务器日志：`/var/log/nginx/qa.tools.com.error.log`
2. Nginx配置：`sudo nginx -t`
3. 服务器状态：`sudo systemctl status nginx`

