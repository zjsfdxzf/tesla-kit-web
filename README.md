# Tesla Kit Web

面向 Tesla 车机浏览器的网页导航与轻量工具站，仿照 [tesla-kit.com](https://www.tesla-kit.com) 的功能，基于开源项目 [BetterTeslaBookmarks](https://gitlab.com/T4cC0re/betterbookmarks) 二次开发。

## 底座来源

核心机制（localStorage 书签持久化 + YouTube redirect 全屏 trick）继承自 BetterTeslaBookmarks：

> Tesla 车机浏览器打开普通网页不会自动全屏，但打开 YouTube 链接会自动全屏。
> 利用 `youtube.com/redirect?q=目标URL` 作为中间跳转，即可借道实现任意网站全屏打开。

在此基础上扩展了 tesla-kit.com 的全部功能。

## 功能对照

| tesla-kit.com 功能 | 本项目实现 |
|---|---|
| 网页导航（常用入口聚合） | 5 大分类：视频娱乐 / 音乐电台 / 出行导航 / 生活工具 / 乘客娱乐 |
| 快捷访问（减少输网址） | 顶部 URL 输入栏，回车即全屏打开 |
| 全屏助手 | 继承 BetterTeslaBookmarks 的 youtube redirect trick |
| 车机 Web 工具（画板） | Canvas 画板，6 色画笔，触控/鼠标均可 |
| 大屏适配 | 深色主题、大卡片大按钮、≥1400px 自动放大触控区域 |
| 使用记录 | localStorage 记录最近 8 条访问，首页快捷 chips |
| 自定义添加网址 | 底部"＋添加网址"按钮，存入"我的收藏"分类 |
| 乘客视频专区（D档播放） | 底部点「📺 乘客看视频」→ 13 个视频网站卡片 → 点击后在页面内 iframe 嵌入播放 |

## 乘客视频专区（D档播放原理）

Tesla 车机浏览器在 D 档（行驶中）会对**顶层页面**的视频播放做黑屏限制，但 **iframe 里的视频不受此限制**。

本项目的乘客视频专区采用 **iframe 嵌入模式**：点击视频网站卡片后，不是跳转到该网站，而是在当前页面内用 `<iframe>` 打开。这样视频在 iframe 里播放，绕过 Tesla 浏览器对顶层页面的 D 档视频限制，乘客即可在行驶中观看。

> 注意：部分大厂网站（YouTube、Netflix、B站等）有 X-Frame-Options 安全头，
> 不允许被 iframe 嵌入，会显示空白。这类网站建议用首页的全屏跳转方式打开。
> 毒舌电影（dushe03.com）等小站没有 X-Frame-Options 限制，可以正常 iframe 嵌入播放。

## 内置视频网站

- **国内**：哔哩哔哩、爱奇艺、腾讯视频、优酷、芒果TV、B站番剧、B站电影、毒舌电影、豆瓣电影、腾讯动漫
- **海外**：YouTube、Netflix、Disney+

## 使用方式

无需构建、无需后端，单文件 `index.html` 直接可用：

```bash
# 本地预览
cd tesla-kit-web
python3 -m http.server 8799
# 浏览器打开 http://localhost:8799
```

车机里使用：把 `http://你的服务器:8799` 加入 Tesla 浏览器书签即可。

## 技术栈

- 纯 HTML + CSS + 原生 JS，零构建零外部依赖
- hash 路由（`#home`、`#board=1`、`#go=URL&fullscreen=1`、`#passenger=host`）
- localStorage 持久化（自定义收藏 `tk_custom`、访问历史 `tk_history`）
- iframe 嵌入模式：乘客视频专区用 iframe 打开视频网站，绕过 D 档视频限制
- 深色主题，车机屏幕低眩光

## 文件结构

```
tesla-kit-web/
├── index.html   # 全部页面（导航/全屏跳转/画板/乘客视频专区）+ 样式 + 逻辑
└── README.md
```

## License

继承 BetterTeslaBookmarks 的开源精神，可自由使用与修改。
