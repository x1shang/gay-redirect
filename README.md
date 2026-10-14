# gay-redirect · 老域名跳转桩

绑定域名：`我们都.是.gay`（punycode `xn--vnqp08bwj8a.xn--0iv.gay`）

2026-10-12 起，导航页本体搬到 **<https://navi.x1shang.top/>**（仓库仍是 `x1shang/navigation-page`）。
这个仓库只做一件事：在老域名上原地接住所有请求，把**路径一起**送到新域名。

| 请求 | 结果 |
| --- | --- |
| `https://我们都.是.gay/` | → `https://navi.x1shang.top/` |
| `https://我们都.是.gay/wetbulb` | → `https://navi.x1shang.top/wetbulb` |
| `https://我们都.是.gay/sorting/?x=1#y` | → `https://navi.x1shang.top/sorting/?x=1#y`（保留 query / hash） |
| `www.我们都.是.gay` | → 老域名（GitHub 默认 www→apex），再同上 |

## 为什么这么做

GitHub Pages 一个仓库只能绑**一个**自定义域名，所以「新域名继续服务 + 老域名做跳转」= 两个仓库：
内容仓（`navigation-page` → `navi.x1shang.top`）+ 跳转桩（本仓库 → 老域名）。
GitHub Pages 只能发静态文件（没有服务端 301），所以跳转是 **JS + `<meta refresh>` 兜底**；
JS 走 `location.pathname + search + hash`，因此子路径不会掉。

JS 先拿 `og-cover.png` 探一下新域名的 https 通不通（证书可能要签发几十分钟）：
通了直接去 https，探不通就先退到 http —— 新域名一旦开了强制 HTTPS，这个 http 会被 301 回 https，
所以退让路径是**自愈**的，等证书签发完不需要再改这里。

## 页面长什么样（2026-10-14 改版）

一屏「**空间曲率引擎启动中**」的深色加载界面：外圈刻度盘 + 双向弧 + 背后雷达扇扫 + 核心光点，
右上角一枚 ONLINE 呼吸灯，底部两根坐标读数（旧坐标 / 跃迁坐标）。**纯 CSS，不引任何外部资源**，
整页 11 KB（含样式与脚本）。

- **刻意让它至少停留 1.1 秒**（脚本里的 `MIN_DWELL`）：不然探测一回来就瞬间跳走，动画根本看不见。想秒跳就把 `MIN_DWELL` 改成 `0`。
- 无障碍：`prefers-reduced-motion` 下动画整体放慢、光点停止闪烁；`<meta refresh>` 留了 3 秒兜底（禁用 JS 时退到新域名首页）。
- 动了 `index.html` 记得**原样拷成 `404.html`**（两份字节一致，这是唯一的维护约定）。

## 文件

```
index.html    老域名根路径（/）的跳转页
404.html      老域名上任何其它路径都由它接管（GitHub Pages 定制 404）→ 也要保留路径
.nojekyll     关掉 Jekyll（下划线文件才不会被吞）
CNAME         老域名（punycode），GitHub Pages 自定义域名的落点
```

改跳转逻辑时 `index.html` 与 `404.html` **必须一起改**。

## DNS（不改）

老域名解析继续指着 GitHub Pages，不用动：

```
@     A      185.199.108.153 / 109 / 110 / 111     （四条，TTL 300）
www   CNAME  x1shang.github.io
_github-pages-challenge-x1shang  TXT  f7e3a8c6fada1b9af312e0ba3b5ae7   （账号级域名验证，永久保留）
```

新域名 `navi.x1shang.top` 那条 CNAME 记在 `x1shang.top` 的 DNS 里（阿里云云解析）：
`navi  CNAME  x1shang.github.io`。
