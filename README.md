# gay-redirect

老域名 `我们都.是.gay`（`xn--vnqp08bwj8a.xn--0iv.gay`）的跳转页，目标为 <https://navi.x1shang.top/>。

采用提供的 fishstew 放大镜页面，支持系统明暗主题、手动切换及减少动态效果。页面加载完成 6 秒后自动跳转；页面中的跳转链接可立即进入目标页。保留原始路径、查询参数和锚点，例如 `/sorting/?x=1#y` → `https://navi.x1shang.top/sorting/?x=1#y`。

禁用 JavaScript 时，6 秒后进入导航页首页，也可使用页面链接。页面只展示跳转效果，不提供实际的人机验证服务。

## 文件与维护

- `index.html`：根路径入口。
- `404.html`：GitHub Pages 子路径入口，必须与 `index.html` 字节一致。
- `assets/`：fishstew 图片、Manrope 字体及其 SIL Open Font License。
- `CNAME`、`.nojekyll`：GitHub Pages 配置。

样式与资源来自提供的 `x1shang-redirect`；吉祥物由 image_gen 基于 fishstew 原图编辑。资源使用根路径，确保任意深度的旧链接都能正常显示页面。

修改 `index.html` 后同步到 `404.html`，推送 `main` 即由现有 GitHub Pages 发布，无需构建或更改 DNS。

## 预览

运行 `python -m http.server 8080` 后，访问 `http://localhost:8080/?preview=1` 暂停自动跳转；加 `&theme=light` 或 `&theme=dark` 指定预览主题。
