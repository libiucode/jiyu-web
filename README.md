# 济语官网 / 下载页（jiyu-web）

仿旦挞官网结构生成的「济语」单页官网：Hero + 特性 + 下载区，静态纯 HTML，无构建依赖，托管在 GitHub Pages，APK 安装包随站分发。

## 本地预览

直接双击 `index.html`，或：

```bash
cd jiyu-web
python -m http.server 8080
# 浏览器打开 http://localhost:8080
```

## 部署到 GitHub Pages（一次性，约 3 分钟）

本机 gh 未登录时按下面步骤手动操作；已登录 `gh auth login` 后有快捷方式（见下文）。

### 1. 创建仓库

在 GitHub 网页右上角 **New repository**：

- Repository name: `jiyu-web`（决定网址：`https://<你的用户名>.github.io/jiyu-web/`）
- 选 **Public**（GitHub Pages 免费版要求公开仓库）
- **不要**勾选 "Add a README"（保持空仓库，避免冲突）

### 2. 推送代码

```bash
cd D:/工作区/2026-10-12-21-47-43/jiyu-web
git init -b main
git add .
git commit -m "济语官网下载页 v1.0"
git remote add origin https://github.com/<你的用户名>/jiyu-web.git
git push -u origin main
```

### 3. 开启 Pages

仓库页面 **Settings → Pages → Build and deployment → Source** 选 **GitHub Actions**。  
推送后 Actions 会自动跑 `Deploy to GitHub Pages`，约 1 分钟后：

> **https://<你的用户名>.github.io/jiyu-web/**

下载地址即为：

> **https://<你的用户名>.github.io/jiyu-web/download/jiyu-v1.0-android.apk**

### 快捷方式（gh 已登录时）

```bash
cd D:/工作区/2026-10-12-21-47-43/jiyu-web
git init -b main && git add . && git commit -m "济语官网下载页 v1.0"
gh repo create jiyu-web --public --source=. --push
gh api -X POST repos/<你的用户名>/jiyu-web/pages -f build_type=workflow
```

## 更新安装包（发新版本时）

方式 A（推荐，简单）：替换 APK 后改版本号再推送

```bash
# 1. 用新包替换 download/jiyu-v1.0-android.apk（建议按版本改名，如 jiyu-v1.1-android.apk）
# 2. 编辑 index.html：更新下载按钮 href、版本号、体积、日期
# 3. 推送
git add . && git commit -m "更新安装包 v1.1" && git push
```

方式 B（更规范，版本化管理）：把 APK 传到 GitHub Releases，页面按钮改为

```
https://github.com/<你的用户名>/jiyu-web/releases/latest/download/jiyu-android.apk
```

（ Releases 资产名固定为 `jiyu-android.apk`，`latest/download` 永远指向最新版，页面无需改动。）

## 注意事项

- 当前安装包为 **debug 签名**的 v1.0（3.9 MB），可正常侧载安装；正式对外发布建议打 release 签名包后再替换。
- GitHub Pages 单文件上限 100 MB，仓库建议 1 GB 以内；APK 变大后请改用 Releases 方式。
- 页面二维码由 CDN 生成，内容为当前页面地址，手机扫码后在手机上打开本页下载。
- 想绑定自己的域名：仓库根目录加一个内容为域名的 `CNAME` 文件，并在域名 DNS 添加 CNAME 记录到 `<你的用户名>.github.io`。

## 推送前建议先改的两处

`index.html` 里有两个占位的 GitHub 链接（下载区「GitHub 源码仓库」和页脚「GitHub」），目前都指向 `https://github.com/`，推送前改成你的仓库地址，例如：

```
https://github.com/<你的用户名>/jiyu-web
```

## 文件结构

```
jiyu-web/
├── index.html                        # 单页官网（唯一页面，含全部样式与脚本）
├── favicon.svg                       # 济语 App 图标
├── download/
│   └── jiyu-v1.0-android.apk         # Android 安装包 v1.0（3.9 MB）
├── .github/workflows/deploy.yml      # 推送 main 自动部署 Pages
└── README.md
```
