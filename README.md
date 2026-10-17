# 云策网络 · 互联网平台运营服务门户

高端门户网页 + 可配置后台，单文件部署，可直接托管在 GitHub Pages。

## 目录结构

| 文件/目录 | 说明 |
|---|---|
| `index.html` | 门户 + 后台（全部功能在一个文件内） |
| `assets/hero.jpg` | 首屏背景图（页面相对引用，需与 index.html 一起发布） |
| `.github/workflows/gh-pages.yml` | GitHub Pages 自动部署配置 |
| `_backup/`、`_shots/` | 本地备份与自检截图（已 gitignore，不会发布） |

## 本地打开

直接双击 `index.html` 即可（Chrome / Edge 推荐）。图片不显示时，请确认 `assets` 文件夹与 `index.html` 在同一目录。

## 进入配置后台

1. 打开页面后，**连点左上角网站 Logo 10 次**（6 秒内）
2. 弹出密码框，输入默认密码 **admin888**
3. 点「进入后台」
4. 后台可配置：品牌、首屏、服务、平台、流程、案例图表、安全、功能配置、联系方式、页脚、主题
5. 修改后点「保存全部」；支持导出 / 导入配置 JSON
6. 密码可在「品牌设置 → 后台访问」修改

## 部署到网站

本文件夹已整理为**可直接拖拽上传**的状态，只包含上线必需内容：

```
platform-ops-portal
├── index.html        （门户 + 后台，已固化你的配置）
├── assets/hero.jpg   （首屏背景图，必须一起上传）
├── README.md
├── .github/          （GitHub 自动部署用，托管平台会忽略）
└── .gitignore
```

### 方法一：Cloudflare Pages 拖拽直传（最快）

1. 打开 `dash.cloudflare.com` → 左侧 **Workers & Pages** → **Create** → **Pages** → 选 **Direct Upload**
2. 把整个 `platform-ops-portal` 文件夹**直接拖进上传区**
3. 输入项目名（如 `zs1234`）→ **Deploy site**
4. 部署完成后即可访问 `https://<项目名>.pages.dev`；以后更新就重新拖入新文件夹（同一项目名）

### 方法二：GitHub 拖拽上传

1. 在 GitHub 新建仓库，进入仓库页面点 **Add file → Upload files**
2. 把整个 `platform-ops-portal` 文件夹拖入（Windows 资源管理器需显示隐藏项目，确保 `.github` 也被选中）
3. Commit 提交后，仓库 → Settings → Pages → Source 选 **GitHub Actions**（或 Deploy from a branch / main / root）

### 方法三：Git 命令行推送（最稳妥）

```bash
git init
git add .
git commit -m "init portal"
git branch -M main
git remote add origin https://github.com/<你的用户名>/platform-ops-portal.git
git push -u origin main
```

仓库里已带 `.github/workflows/gh-pages.yml`，推送后自动发布。

## 修改内容后如何同步到线上

后台的配置默认保存在**浏览器本地（localStorage）**，它不会跟随文件上传到 GitHub——这就是"线上打开还是旧样子"的原因。当前版本已把你导出的配置**固化进 index.html 的内置配置**（`EMBEDDED_CONFIG`），所有人打开都会看到这份内容。

以后想再改内容并同步线上：

1. 本地打开页面 → 连点 Logo 10 次进入后台 → 修改内容 → 点「保存全部」
2. 点「导出配置」下载 JSON
3. 两种固化方式任选：
   - 把 JSON 文件发给维护方（或豆包），由 TA 重新固化后回传 `index.html`
   - 自己动手：用文本编辑器打开 `index.html`，找到 `var EMBEDDED_CONFIG={ ... };` 整段，替换为你导出的 JSON 内容（直接粘贴即可），保存
4. 用新的 `index.html` 替换本地文件后重新 push，线上即更新

注意：后台「恢复默认」现在恢复的是内置发布配置（即固化版本），不是出厂初始值。

## 注意事项

- 后台密码与配置保存在浏览器 localStorage（本机），换设备 / 换浏览器需重新配置；正式上线建议增加服务端存储与鉴权
- 图片为相对路径引用，发布时务必包含 `assets` 文件夹
