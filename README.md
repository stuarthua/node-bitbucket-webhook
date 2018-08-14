# node-bitbucket-webhook

Bitbucket Webhooks handler based on `Node.js`. Support multiple handlers.

## Language

- [中文](docs/zh_CN.md)

## Instructions

This library is inspired by [github-webhook-handler](https://github.com/rvagg/github-webhook-handler), and it allows you set multiple handlers for different repositories.

It is a small tool based on Node.js to help you handler all the logic for receiving and verifying webhook requests from Bitbucket.

If you want to know webhooks of Bitbucket, please see: [Manage webhooks](https://confluence.atlassian.com/bitbucket/manage-webhooks-735643732.html).

Notice: `Content-type` - `application/json`.

## Installation

`npm install node-bitbucket-webhook --save`

## Usage

```js
var http = require('http')
var createHandler = require('node-bitbucket-webhook')
var handler = createHandler([ // multiple handlers
  { path: '/webhook1' },
  { path: '/webhook2' }
])
// var handler = createHandler({ path: '/webhook1' }) // single handler

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
  switch(event.path) {
    case '/webhook1':
      // do sth about webhook1
      break
    case '/webhook2':
      // do sth about webhook2
      break
    default:
      // do sth else or nothing
      break
  }
})
```
