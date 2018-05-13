# Websockets 简介

> Websockets 是 Web 应用程序中 HTTP 通信的替代方案。它可以为客户端和服务端之间提供一个实时的双向通信通道。了解如何使用它来进行网络交互。

<!-- TOC -->

- [Websockets 简介](#websockets-简介)
  - [浏览器对 WebSockets 的支持](#浏览器对-websockets-的支持)
  - [Websockets 和 HTTP 的不同](#websockets-和-http-的不同)
  - [安全的 Websockets](#安全的-websockets)
  - [创建 Websockets 连接](#创建-websockets-连接)
  - [使用 Websockets 向服务端发送数据](#使用-websockets-向服务端发送数据)
  - [使用 Websockets 接收服务端返回的数据](#使用-websockets-接收服务端返回的数据)
  - [使用 Node.js 实现服务端](#使用-nodejs-实现服务端)
  - [在 Glitch 上查看示例](#在-glitch-上查看示例)

<!-- /TOC -->

Websockets 是 Web 应用程序中 HTTP 通信的替代方案。

它可以为客户端和服务端之间提供一个实时的双向通信通道。

一旦建立连接，通道将保持打开状态，从而提供一个具有低延迟和低开销的连接。

## 浏览器对 WebSockets 的支持

所有现代浏览器都支持 Websockets 。

![Browser support for WebSockets](https://raw.githubusercontent.com/coderfe/100-days-of-translate/master/introduction-to-websockets/1.png)

## Websockets 和 HTTP 的不同

HTTP 是一种非常不同的协议，也是一种不同的通信方式。

HTTP 是一种 request/response 协议：当客户端发送请求时，服务端会返回一些数据。

而 Websockets ：

- 服务端可以向客户端发送信息而不需要客户端明确地发出请求。
- 客户端和服务端彼此可以同时通信。
- 发送消息所需开销很小。这意味着低延迟通信。

Websockets 非常适合实时和长时间的通信。

HTTP 适用于偶尔的数据交换，以及客户端主动发起的交互。

HTTP 的实现较为简单，而 Websockets 则需要更多的开销。

## 安全的 Websockets

始终使用安全的、加密的 Websockets ：`wss://` 。

`ws://` 是指不安全的 Websockets 版本（相当于 Websockets 的 `http://`），因此应该避免使用。

## 创建 Websockets 连接

```javascript
const url = 'wss://myserver.com/something';
const connection = new Websocket(url);
```

`connection` 是一个 [Websocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) 对象。

当连接建立成功后会触发 `open` 事件。

通过为 `connection` 对象的 `onopen` 属性指定回调函数来监听这一事件：

```javascript
connection.onopen = () => {
  //...
}
```

如果有错误发生将会触发 `onerror` 的回调函数：

```javascript
connection.onerror = (error) => {
  console.log(`Websocket error: ${error}`);
}
```

## 使用 Websockets 向服务端发送数据

当连接打开时，你可以向服务端发送数据。

你可以很容易地在 `onopen` 的回调函数里执行操作：

```javascript
connection.onopen = () => {
  connection.send('data');
}
```

## 使用 Websockets 接收服务端返回的数据

使用 `onmessage` 的回调函数进行监听，收到消息时会触发这个函数：

```javascript
connection.onmessage = (e) => {
  console.log(e.data);
}
```

## 使用 Node.js 实现服务端

[ws](https://github.com/websockets/ws) 是一个在 [Node.js](https://flaviocopes.com/nodejs/) 中很受欢迎的 Websockets 库。

我们将使用它来搭建一个 Websockets 服务端。它也可以用来实现客户端，并使用 Websockets 在两个后端服务器之间通信。

用下面的方式轻松地安装：

```shell
yarn init
yarn add ws
```

需要你自己写的代码很少：

```javascript
const Websocket = require('ws');

const wss = new Websocket.Server({ port: 8080 });
wss.on('connection', (ws) => {
  ws.on('message', (message) => {
    console.log(`Received message => ${message}`);
  })
  ws.send('ho!');
});
```

这些代码在 8080 端口（Websockets 的默认端口）创建了一个新的服务，并在连接建立时添加了一个回调函数，向客户端发送 `ho!` ，并且记录了收到的信息。

## 在 Glitch 上查看示例

这是 Websockets 服务端的示例：[ https://glitch.com/edit/#!/flavio-websockets-server-example]( https://glitch.com/edit/#!/flavio-websockets-server-example)

这是客户端和服务端通过 Websockets 交互的示例：[https://glitch.com/edit/#!/flavio-websockets-client-example](https://glitch.com/edit/#!/flavio-websockets-client-example)
