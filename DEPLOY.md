# SIMDT 官网部署指南

## 当前状态

- 官网代码已就绪：`simdt-website/` 目录
- Git 仓库已初始化，已提交初始 commit
- CloudStudio 线上预览已部署
- GitHub Pages 部署：需要 VPN（当前网络无法直连 GitHub）

---

## GitHub Pages 部署步骤（需要 VPN）

### 第 1 步：安装 GitHub CLI

打开 PowerShell（管理员），运行：

```powershell
winget install --id GitHub.cli --exact
```

安装后重启终端，确认：

```powershell
gh --version
```

### 第 2 步：登录 GitHub

```powershell
gh auth login
```

按提示选择：
- GitHub.com
- HTTPS
- 用浏览器登录（会打开 GitHub 授权页面）

### 第 3 步：创建仓库并推送

进入项目目录：

```powershell
cd G:\workbuddy\2026-07-07-11-16-51\simdt-website
```

创建 GitHub 仓库并推送（仓库会设为 public，GitHub Pages 免费版需要 public）：

```powershell
gh repo create simdt-website --public --source=. --push
```

### 第 4 步：启用 GitHub Pages

```powershell
gh api repos/{owner}/simdt-website/pages -X POST -f "source[branch]=main" -f "source[path]=/"
```

或者手动操作：
1. 打开 `https://github.com/<你的用户名>/simdt-website/settings/pages`
2. Source 选 **Deploy from a branch**
3. Branch 选 **main** / **/(root)**
4. 点 Save

### 第 5 步：绑定自定义域名

CNAME 文件已在仓库中（内容为 `www.simdt.cn`），GitHub Pages 会自动识别。

还需要去你的域名 DNS 管理后台添加记录：

| 记录类型 | 主机记录 | 记录值 |
|----------|----------|--------|
| CNAME | www | `<你的用户名>.github.io` |

DNS 生效后（通常几分钟到几小时），访问 `https://www.simdt.cn` 即可看到官网。

### 第 6 步（可选）：启用 HTTPS

在 `https://github.com/<你的用户名>/simdt-website/settings/pages` 页面：
- 勾选 **Enforce HTTPS**

GitHub 会自动为自定义域名签发 SSL 证书。

---

## 后续更新网站

代码改完后，一行命令推上去：

```powershell
cd G:\workbuddy\2026-07-07-11-16-51\simdt-website
git add -A && git commit -m "update website" && git push
```

GitHub Pages 会在 1-2 分钟内自动更新。

---

## 文件结构

```
simdt-website/
├── index.html          # 主页面（含全部 CSS/JS/i18n 数据）
├── CNAME               # GitHub Pages 自定义域名配置
├── favicon.ico         # 网站图标
└── images/
    ├── logo.png                 # SIMDT logo
    ├── product-haptic.jpg       # 触觉反馈马达
    ├── product-wind.jpg         # 风力模拟系统
    ├── product-wheel.jpg        # GT 方向盘
    ├── product-buttonbox.jpg    # 按钮盒
    └── product-dashboard.jpg    # 5寸触摸屏仪表盘
```
