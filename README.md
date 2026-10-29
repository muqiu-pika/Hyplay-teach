# Hyplay-teach

Hyplay MC 服务器的进服教程文件存放处。

## 内容

| 路径 | 说明 |
| --- | --- |
| `media/hyplayteach.mp4` | 进服教程视频（1920×1080，H.264/AAC，5:58，72 MB） |
| `index.html` | 播放页，含一键复制直链 / 嵌入代码 |
| `vercel.json` | Vercel 配置（静态资源长缓存 + 允许跨域） |

## 视频直链

部署到 Vercel 后，视频通过下面的地址访问：

```
https://<你的域名>/media/hyplayteach.mp4
```

`media/` 下的资源由 `vercel.json` 设置了 `Cache-Control: public, max-age=31536000, immutable`
与 `Access-Control-Allow-Origin: *`，可以直接跨域引用。

> 视频内容若更新，**请换成新的文件名**（例如 `hyplayteach-v2.mp4`）并同步修改 `index.html`，
> 否则旧缓存（浏览器 + Vercel 边缘节点）会继续返回旧文件。

## 嵌入到其他网页

最小可用写法：

```html
<video controls playsinline preload="metadata"
       width="100%" style="max-width:900px;border-radius:12px"
       src="https://<你的域名>/media/hyplayteach.mp4"></video>
```

想连播放页一起嵌入（自带标题和复制面板）：

```html
<iframe src="https://<你的域名>/"
        width="900" height="600"
        style="border:0;border-radius:12px;max-width:100%"
        allowfullscreen></iframe>
```

Markdown / 富文本编辑器（不支持 `<video>` 时）：

```markdown
[点击观看进服教程](https://<你的域名>/media/hyplayteach.mp4)
```

## 部署

仓库是无构建步骤的纯静态站点，Vercel 零配置直接托管：

1. 在 <https://vercel.com/new> 导入本仓库，框架选 **Other**，Build Command 与 Output Directory 均留空。
2. 部署完成即可访问 `https://<project>.vercel.app`，并自动获得上述直链。
3. 之后每次 `git push` 到 `main` 都会自动重新部署。

## 注意事项

- **体积**：Vercel Hobby 单次部署的静态文件上传上限为 100 MB，当前 72 MB 在范围内。
- **流量**：Hobby 每月约 100 GB 快速数据传输，按 72 MB/次计算约合 1400 次完整播放。播放量上去后建议换用对象存储（R2 / OSS）或视频平台。
- **分片与拖拽**：Vercel CDN 原生支持 `Range` 请求，视频可拖动进度条；源文件 `moov` 位于 `mdat` 之前（faststart），首帧出图更快。
