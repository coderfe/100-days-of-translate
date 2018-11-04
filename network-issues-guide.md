# 「Chrome Devtools」网络问题指南

## 排队或停滞的请求

### 症状

6 个请求在同时下载，在这之后，又有一系列的请求在排队或停滞。一旦 6 个请求中的其中 1 个完成时，
队列中的其中 1 个请求也会立即开始。

![排队或停滞的请求][stalled]

图 1：Network 面板中排队或停滞的请求的示例。在 **Waterfall** 中你可以看到
`logo-1024px.png` 的六个请求同时开始了。之后的请求被挂起，直到上面的 6 个请求中完成 1 个。

### 原因

对一个域名发出太多请求。在 HTTP/1.0 或 HTTP/1.1 连接中，
Chrome 允许与每台主机同时只能有 6 个请求。

### 修复

- 如果你必须使用 HTTP/1.0 或者 HTTP/1.1，则可以实现域名分片。
- 使用 HTTP/2，但是不要和域名分片一起使用。
- 移除或延迟不必要的请求，以便关键请求可以优先下载。

## Slow Time To First Byte（TTFB）

### 症状

1 个请求花费了相当长的时间来等待接收从服务器发出的第一个字节。

![ttfb][ttfb]

图 2：TTFB 的请求示例。**Waterfall** 中绿色的长条显示出 `wait` 请求等待了相当长的一段时间。

### 原因

- 客户端和服务端的连接很慢。
- 服务端的响应较慢。将服务器托管在本地以确定是连接慢还是服务器慢。如果你本地的 TTFB 依旧较慢，
那就说明是服务器比较慢。

### 修复

- 如果连接比较慢，考虑将内容托管在 CDN 上，或者更换托管服务商。
- 如果服务器比较慢，考虑优化数据库查询，实现缓存，或者修改服务器配置。

## Slow content download

### 症状

一个请求花费长时间来下载。

![slow-content-download][slowcontentdownload]

图 3：花费长时间下载请求的示例。**Waterfall** 中蓝色的长条意味着下载 `elements-panel.png`
花费了很长时间。

### 原因

- 客户端和服务端的连接很慢。
- 许多内容正在下载。

### 修复

- 考虑将内容托管到 CDN，或者更换托管服务商。
- 通过优化请求发送更少的内容。

[stalled]:https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/network-issues-guide/stalled.png
[ttfb]:https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/network-issues-guide/slow-ttfb.png
[slowcontentdownload]:https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/network-issues-guide/slow-content-download.png
