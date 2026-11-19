# 签到助手 · PWA 版

可以「安装」到手机桌面的网页应用：有独立图标、全屏无地址栏、离线可用，数据存本机。
功能与单文件版 `../checkin.html` 完全一致。

## 目录

```
checkin-app/
├── index.html            应用本体（由 ../checkin.html 生成，见末尾说明）
├── manifest.webmanifest  应用信息：名称、图标、全屏方式
├── sw.js                 Service Worker：离线缓存 + 自动更新
└── icons/                192 / 512 / maskable / apple-touch 图标
```

## 第一步：必须放到一个网址上

PWA 的「安装」只在 **https**（或电脑上的 localhost）下有效，直接双击本地文件是装不了的。
任选一种免费托管，把 `checkin-app` **整个文件夹**传上去：

### 方案 A：Cloudflare Pages（推荐，国内一般可访问）

1. 注册 https://dash.cloudflare.com
2. 左侧 **Workers 和 Pages** → **创建** → **Pages** → **上传资产**
3. 把 `checkin-app` 文件夹整个拖进去 → 部署
4. 得到形如 `https://checkin-xxx.pages.dev` 的网址

### 方案 B：Netlify Drop

1. 打开 https://app.netlify.com/drop
2. 拖入 `checkin-app` 文件夹
3. 拿到网址（登录后站点才会永久保留）

### 方案 C：GitHub Pages

1. 新建仓库 → 上传文件夹内容 → **Settings → Pages** 选择分支
2. 网址形如 `https://用户名.github.io/仓库名/`
3. 国内访问可能较慢

## 第二步：装到手机

**安卓（Chrome / Edge）**
打开网址 → 右上角 `⋮` → **安装应用**（或「添加到主屏幕」）→ 桌面出现「签到」图标

**iPhone（必须用 Safari）**
Safari 打开网址 → 底部 **分享** 按钮 → **添加到主屏幕** → 添加

装好后从桌面图标进入：全屏、无地址栏，断网也能打开和记录。

## 本地预览（电脑上）

```bash
# 在 checkin-app 目录里
python -m http.server 8000
# 浏览器打开 http://localhost:8000
```

`localhost` 属于安全上下文，可以看到安装提示、也能验证离线缓存。

## 更新

改了 `index.html` 直接覆盖线上文件即可：页面导航是**网络优先**，用户下次联网打开就是新版；
离线时继续用缓存里的旧版，联网后自动更新。

如果想让离线缓存也立刻换新，把 `sw.js` 里的 `VERSION` 改一下（如 `checkin-v2`）。

## 数据

- 存在浏览器的 `localStorage`，键名 `checkin_tasks_v1`
- 换浏览器、清网站数据、换设备都会丢，**没有云同步**
- 需要备份的话可以再加「导出 / 导入 JSON」

## 备注

`index.html` 是由 `../checkin.html` 生成的：在单文件版基础上补了 manifest、图标和 Service Worker 注册。
要改功能，改 `checkin.html`，然后重新生成一份到 `checkin-app/index.html`，两边保持一致。
