---
title: "Express学习前端笔记"
date: 2025-03-13T14:30:00+08:00
author: "Eira Hazel"
language: zh-cn
draft: false
categories:
- 技术探讨
tags:
- Node.js
- Web开发
keywords:
- Express路由
- 中间件机制
- RESTful API
---

* Express搭前端测试环境
- 装express：pnpm add express
- 新建server.js ↓

// 最简启动
const express = require('express')
const app = express()
app.use(express.static('public'))  ← 静态资源扔这个文件夹
app.listen(3000)

* 文件夹结构：
/public
  |- index.html
  |- style.css
  |- app.js
/server.js

* 动态路由模拟数据 ← 假装有后端API
// 在server.js加这个
app.get('/api/user', (req, res) => {
  res.json({ 
    name: '临时数据',
    id: Date.now() 
  })
})

// 前端js里用fetch调用
fetch('/api/user')
  .then(res => res.json())
  .then(data => console.log(data))

> 注意！！解决跨域问题：
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*')
  next()
})

* 路由history模式支持 ← 用这个让前端路由不出现#号
const history = require('connect-history-api-fallback')
app.use(history())  ← 加在static中间件前面

（记得装包 pnpm add connect-history-api-fallback）

* 热更新小技巧
- 用nodemon监控文件改动
- package.json里加：
"scripts": {
  "dev": "nodemon server.js"
}

（下次试试用vite更好？不过express手动配更透明）