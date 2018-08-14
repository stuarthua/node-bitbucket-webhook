# node-bitbucket-webhook

基于 `Node.js` 的 Bitbucket Webhooks 工具, 支持设置多个仓库。

## 介绍

这个库受到[github-webhook-handler](https://github.com/rvagg/github-webhook-handler)启发而来，它支持你为多个仓库同时设置钩子。

该库基于 `Node.js` ，能帮你处理所有来自Github的webhooks请求。

如果你想要了解 Bitbucket 的 Webhooks ，请看：[Manage webhooks](https://confluence.atlassian.com/bitbucket/manage-webhooks-735643732.html)。

注意：`Content-type` - `application/json`。

## 安装

`npm install node-bitbucket-webhook --save`

## 使用

```js
var http = require('http')
var createHandler = require('node-bitbucket-webhook')
var handler = createHandler([ // 多个仓库
  { path: '/webhook1' },
  { path: '/webhook2' }
])
// var handler = createHandler({ path: '/webhook1' }) // 单个仓库

http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(7777)

handler.on('error', function (err) {
  console.error('Error:', err.message)
})

handler.on('push', function (event) {
  console.log(
    'Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref
  )
  switch (event.path) {
    case '/webhook1':
      // 处理webhook1
      break
    case '/webhook2':
      // 处理webhook2
      break
    default:
      // 处理其他
      break
  }
})
```
