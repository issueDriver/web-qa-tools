# 项目结构说明

## 📂 目录结构

```
p5/
├── index.html                    # 首页入口文件
├── README.md                     # 项目说明文档
├── PROJECT_STRUCTURE.md         # 项目结构说明（本文件）
├── tech.txt                      # 技术需求文档
├── .gitignore                    # Git忽略文件配置
│
├── pages/                        # 工具页面目录
│   ├── tax-calculator.html       # 税率计算器
│   ├── timestamp.html            # 时间戳工具
│   ├── data-generator.html       # 测试数据生成器
│   ├── qrcode.html               # 二维码/一维码生成
│   ├── qrcode-reader.html        # 二维码识别
│   ├── json-formatter.html       # JSON格式化
│   ├── text-diff.html            # 文本对比
│   ├── map-coordinate.html       # 地图经纬度工具
│   ├── image-processor.html      # 图片处理工具
│   ├── ssl-checker.html          # SSL证书检测
│   └── extension.html             # 扩展工具
│
└── assets/                       # 静态资源目录
    └── icons/                    # 图标文件目录
        ├── icon.svg              # SVG格式图标
        └── favicon.ico           # 网站图标
```

## 📋 文件说明

### 根目录文件

- **index.html**: 项目首页，工具集合入口页面
- **README.md**: 项目说明和使用文档
- **tech.txt**: 原始技术需求文档
- **.gitignore**: Git版本控制忽略文件配置

### pages/ 目录

所有工具页面统一存放在 `pages/` 目录下，便于管理和维护。

### assets/ 目录

静态资源文件统一存放在 `assets/` 目录下：
- **icons/**: 存放图标文件（SVG、ICO格式）

## 🔗 路径引用规则

### 从首页到工具页面
```html
<a href="pages/tool-name.html">工具名称</a>
```

### 从工具页面返回首页
```html
<a href="../index.html">← 返回首页</a>
```

### 图标资源引用
```html
<link rel="icon" type="image/svg+xml" href="assets/icons/icon.svg">
<img src="assets/icons/icon.svg" alt="图标">
```

## 🎯 设计原则

1. **分离关注点**: HTML页面与静态资源分离
2. **统一管理**: 所有工具页面集中管理
3. **易于维护**: 清晰的目录结构便于后续扩展
4. **标准化**: 遵循Web项目标准目录结构

## 📝 后续扩展

如需添加新工具：
1. 在 `pages/` 目录下创建新的HTML文件
2. 在 `index.html` 中添加工具卡片链接
3. 确保返回首页链接使用 `../index.html`

如需添加新资源：
- CSS文件 → `assets/css/`
- JavaScript文件 → `assets/js/`
- 图片文件 → `assets/images/`
- 图标文件 → `assets/icons/`

