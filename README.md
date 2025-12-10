# 工具集合

一个实用的在线工具集合，包含多种常用工具，提高工作效率。

## 📁 项目结构

```
p5/
├── index.html              # 首页入口
├── pages/                  # 工具页面目录
│   ├── tax-calculator.html      # 税率计算器
│   ├── timestamp.html           # 时间戳工具
│   ├── data-generator.html      # 测试数据生成器
│   ├── qrcode.html              # 二维码/一维码生成
│   ├── qrcode-reader.html       # 二维码识别
│   ├── json-formatter.html      # JSON格式化
│   ├── text-diff.html           # 文本对比
│   ├── map-coordinate.html      # 地图经纬度工具
│   ├── image-processor.html    # 图片处理工具
│   ├── ssl-checker.html         # SSL证书检测
│   ├── regex-tester.html        # 正则表达式工具
│   ├── gantt-chart.html         # 甘特图工具
│   ├── url-builder.html         # URL拼接工具
│   ├── text-formatter.html      # 文本格式化工具
│   └── extension.html           # 扩展工具
├── assets/                 # 资源文件目录
│   └── icons/              # 图标文件
│       ├── icon.svg        # SVG图标
│       └── favicon.ico     # 网站图标
├── README.md               # 项目说明文档
├── DEPLOYMENT.md           # 部署指南
├── nginx.conf.example      # Nginx配置示例
├── Dockerfile              # Docker镜像配置
├── docker-compose.yml      # Docker Compose配置
├── deploy.sh               # 部署脚本
├── CNAME                   # GitHub Pages自定义域名配置
└── tech.txt                # 技术需求文档
```

## 🛠️ 工具列表

### 1. 税率计算器
- 计算含税金额、不含税金额和税额
- 支持双向计算（含税/不含税）

### 2. 时间戳工具
- 时间戳与日期时间相互转换
- 时间计算（加减天数、小时等）
- 多种时间格式输出
- 相对时间显示

### 3. 测试数据生成器
- 生成姓名、身份证号、手机号
- 生成商品名称（燃气配件、液化气）
- 生成优惠券名称
- 支持批量生成

### 4. 二维码/一维码生成
- 生成二维码（支持多种格式）
- 生成一维码（条形码）
- 自定义颜色、尺寸、纠错级别

### 5. 二维码识别
- 图片识别二维码
- 摄像头实时扫描
- 支持粘贴图片

### 6. JSON格式化
- JSON格式化与美化
- JSON压缩
- JSON验证
- 支持复制功能

### 7. 文本对比
- 对比两段文本差异
- 字符级别高亮显示
- 显示统计信息

### 8. 地图经纬度工具
- 地图坐标采集
- 地址转经纬度
- 坐标系转换（WGS84、GCJ02、BD09）
- 历史记录功能

### 9. 图片处理工具
- 图片压缩（可调质量）
- 图片放大（增加文件大小）
- 等比例裁剪
- 支持多种格式

### 10. SSL证书检测
- 检测网站SSL证书信息
- 显示证书有效期
- 证书状态检查

### 11. 正则表达式工具
- 正则表达式测试和验证
- 实时匹配结果高亮显示
- 文本替换功能
- 常用正则表达式模板
- 支持捕获组显示

### 12. 甘特图工具
- 创建和管理项目任务
- 可视化时间线和进度
- 自定义任务颜色和进度
- 导出JSON和图片
- 本地存储数据

### 13. URL拼接工具
- 快速拼接URL参数
- 支持自定义基础URL
- 实时预览生成的URL
- 一键复制到剪贴板
- 在新窗口打开链接

### 14. 文本格式化工具
- 将多行编号内容转换为单行平铺格式
- 自动去除换行符
- 支持中文编号格式（1、2、3、）
- 支持数字编号格式（1. 2. 3.）
- 一键复制格式化结果

## 🚀 使用方法

### 本地使用

1. 直接在浏览器中打开 `index.html`
2. 点击相应的工具卡片进入工具页面
3. 使用完工具后点击"返回首页"回到主页

### 部署到服务器

详细部署指南请参考 [DEPLOYMENT.md](./DEPLOYMENT.md)

**快速部署（Nginx）：**

```bash
# 1. 上传文件到服务器
scp -r * user@server:/var/www/qa.tools.com/

# 2. 配置Nginx（参考 nginx.conf.example）
sudo cp nginx.conf.example /etc/nginx/sites-available/qa.tools.com
sudo ln -s /etc/nginx/sites-available/qa.tools.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# 3. 配置SSL证书（可选但推荐）
sudo certbot --nginx -d qa.tools.com

# 4. 配置DNS
# 添加A记录: qa -> 服务器IP地址
```

**使用Docker部署：**

```bash
docker-compose up -d
```

## 📝 技术特点

- 纯HTML5实现，无需后端服务器
- 响应式设计，支持移动端
- 所有处理在浏览器本地完成，保护隐私
- 使用现代Web API（Canvas、Geolocation、Clipboard等）

## 📄 许可证

本项目仅供学习和个人使用。

