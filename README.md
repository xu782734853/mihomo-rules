# Mihomo MRS 规则自动转换器

🤖 使用 GitHub Actions 自动将文本规则转换为二进制 MRS 格式

## 📋 功能特点

- ✅ 自动转换文本规则为二进制 MRS 格式
- ✅ 支持多种规则类型（domain、ipcidr、classical）
- ✅ 每次提交自动触发转换
- ✅ 自动发布 Release 供订阅使用
- ✅ 无需本地安装 mihomo 工具

## 🚀 快速开始

### 1. Fork 或创建仓库

点击右上角 `Use this template` 或 Fork 本仓库

### 2. 添加规则文件

在 `rules/text/` 目录下创建或编辑文本规则文件：

```
rules/
  text/
    domain.txt           # 域名规则
    domain_suffix.txt    # 域名后缀规则
    domain_keyword.txt   # 域名关键字规则
    ipcidr.txt          # IP 地址段规则
```

**文件格式示例：**

`domain.txt`:
```
google.com
youtube.com
facebook.com
```

`ipcidr.txt`:
```
192.168.0.0/16
10.0.0.0/8
```

### 3. 提交触发转换

```bash
git add rules/text/*.txt
git commit -m "更新规则"
git push
```

GitHub Actions 会自动：
1. 下载最新版 mihomo
2. 将所有 `.txt` 文件转换为 `.mrs`
3. 创建 Release 发布
4. 生成订阅链接

### 4. 订阅使用

转换完成后，在 Release 页面找到生成的 `.mrs` 文件链接，配置到 mihomo：

```yaml
rule-providers:
  my-domain:
    type: http
    behavior: domain
    format: mrs
    url: "https://github.com/你的用户名/你的仓库/releases/latest/download/domain.mrs"
    interval: 86400
```

## 📁 目录结构

```
.
├── .github/
│   └── workflows/
│       └── convert.yml      # GitHub Actions 工作流
├── rules/
│   ├── text/               # 存放文本规则（手动编辑）
│   │   ├── domain.txt
│   │   └── ipcidr.txt
│   └── mrs/                # 存放转换后的 MRS（自动生成）
│       ├── domain.mrs
│       └── ipcidr.mrs
├── scripts/
│   └── convert.sh          # 转换脚本
└── README.md
```

## 🛠️ 规则类型说明

| 文件名 | 规则类型 | 说明 | behavior |
|--------|---------|------|----------|
| `domain.txt` | DOMAIN | 精确域名匹配 | domain |
| `domain_suffix.txt` | DOMAIN-SUFFIX | 域名后缀匹配 | domain |
| `domain_keyword.txt` | DOMAIN-KEYWORD | 域名关键字匹配 | domain |
| `ipcidr.txt` | IP-CIDR | IPv4/IPv6 地址段 | ipcidr |
| `classical.txt` | 混合规则 | 完整规则格式 | classical |

## 🔧 高级配置

### 自定义转换规则

编辑 `.github/workflows/convert.yml`，修改转换参数：

```yaml
- name: 转换规则
  run: |
    ./mihomo convert-ruleset domain text rules/text/domain.txt rules/mrs/domain.mrs
    ./mihomo convert-ruleset ipcidr text rules/text/ipcidr.txt rules/mrs/ipcidr.mrs
```

### 定时自动更新

在 workflow 中添加 schedule 触发器：

```yaml
on:
  push:
    paths:
      - 'rules/text/**'
  schedule:
    - cron: '0 0 * * *'  # 每天 00:00 UTC 自动运行
```

## 📝 示例规则

### domain.txt
```
# Google 服务
google.com
googleapis.com
gstatic.com
youtube.com

# 社交媒体
facebook.com
twitter.com
instagram.com
```

### ipcidr.txt
```
# 局域网
192.168.0.0/16
10.0.0.0/8
172.16.0.0/12

# 特定 IP 段
1.1.1.0/24
8.8.8.0/24
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License
