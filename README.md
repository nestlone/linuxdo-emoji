# Linux.do Market Emoji Picker

一个为 [Linux.do](https://linux.do/) 设计的用户脚本。它将云端表情包市场接入回复与聊天编辑器，支持分组选择、收藏、离线缓存和高性能浏览。

当前版本：`0.0.1`

## 安装

请先安装任一支持用户脚本的浏览器扩展：Tampermonkey、Violentmonkey 或 ScriptCat。

[安装或更新脚本](https://raw.githubusercontent.com/nestlone/linuxdo-emoji/main/market-emoji-picker.user.js)

安装后打开 Linux.do 的回复或聊天编辑器，点击工具栏中的表情按钮即可使用。

## 功能

- 从云端市场浏览并组合表情包分组。
- 搜索当前已选分组中的表情。
- 右键点击或长按表情，加入或移出收藏夹。
- 支持 Markdown 和 HTML 两种插入格式，以及图片缩放比例切换。
- 支持悬浮大图预览和移动端底部弹窗。
- 支持脚本内检查更新与用户脚本管理器自动更新。

## 显示与性能

表情选择器针对大量图片和动图做了以下处理：

- 可视区域虚拟化：只创建当前可见区域附近的表情节点，降低大分组滚动时的 DOM 与内存压力。
- 限流下载：图片请求共享队列，并限制并发，避免打开选择器时抢占页面资源。
- 单次下载复用：同一图片下载得到的 Blob 同时用于显示和离线缓存，避免重复网络请求。
- 动图兼容：为跨域回退下载的 GIF、WebP、AVIF 补齐 MIME 类型，改善动画表情显示。
- 空闲预热：仅在选择器已打开且浏览器空闲时预热常用图片。

## 本地缓存

图片缓存保存在浏览器的 IndexedDB；市场元数据和已选分组保存在当前站点的 Local Storage。

默认缓存策略：

- 最多 500 张图片。
- 最大占用 100 MB。
- 每 7 天清理一次从未实际插入使用的缓存。
- 超出限制时，优先清理使用次数最少、且较久未访问的图片。

在用户脚本菜单中选择“💾 设置离线图片缓存上限”可以修改数量和容量；选择“🗑️ 清除所有本地缓存与离线图片”可以立即清空缓存。

## 使用说明

1. 点击编辑器工具栏中的表情按钮打开选择器。
2. 点击“⚙️”进入市场管理，选择需要的表情包并保存。
3. 通过顶部标签切换分组，或使用搜索框查找表情。
4. 点击表情插入编辑器；右键或长按可以收藏。
5. 点击底部版本号可手动检查更新。

## 常见问题

**动图显示为空白或异常闪烁怎么办？**

刷新 Linux.do 页面以加载最新脚本；若问题仍存在，请通过用户脚本菜单清除离线图片缓存后重新打开选择器。

**缓存保存在哪里？**

缓存属于浏览器的 Linux.do 站点数据，而不是脚本所在文件夹。可在浏览器开发者工具的 Application/应用程序面板中查看 IndexedDB 和 Local Storage。

**为什么有些图片首次显示较慢？**

首次访问需要从远程图片源下载；后续使用会优先命中本地缓存。若市场数据提供 `thumbnailUrl`、`thumbUrl` 或 `previewUrl`，网格会优先使用它们作为缩略图。

## 开发

项目只有一个可安装脚本文件：

- `market-emoji-picker.user.js`：用户脚本源码。

提交修改前请至少执行：

```powershell
node --check market-emoji-picker.user.js
git diff --check
```

欢迎通过 [Issues](https://github.com/nestlone/linuxdo-emoji/issues) 报告问题或提交改进建议。

## 链接

- [项目主页](https://github.com/nestlone/linuxdo-emoji)
- [最新脚本](https://raw.githubusercontent.com/nestlone/linuxdo-emoji/main/market-emoji-picker.user.js)

## 许可证

本项目采用 MIT License。
