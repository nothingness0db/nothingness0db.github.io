---
title: "Express learning front-end notes"
date: 2025-03-13T14:30:00+08:00
author: "Eira Hazel"
language: en
draft: false
categories:
- Technical Discussion
tags:
- Node.js
- Web Development
keywords:
- Express Routing
- Middleware Mechanism
- RESTful API
---



* Setting up a frontend test environment with Express is super convenient
- Install Express: `pnpm add express`
- Create server.js ↓

```javascript
// Minimal setup to start
const express = require('express')
const app = express()
app.use(express.static('public'))  ← Static assets go here
app.listen(3000)
```

* Folder structure:
```
/public
  |- index.html
  |- style.css
  |- app.js
/server.js
```

* Mock API endpoints ← Simulate backend data
```javascript
// Add this to server.js
app.get('/api/user', (req, res) => {
  res.json({ 
    name: 'Temporary data',
    id: Date.now() 
  })
})

// Frontend fetch usage
fetch('/api/user')
  .then(res => res.json())
  .then(data => console.log(data))
```

> Important!! Fix CORS issues:
```javascript
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*')
  next()
})
```

* History mode routing support ← Remove hash (#) from URLs
```javascript
const history = require('connect-history-api-fallback')
app.use(history())  ← Place before static middleware
```
(Remember to install: `pnpm add connect-history-api-fallback`)

* Hot reload trick
- Use nodemon to monitor file changes
- Add to package.json:
```json
"scripts": {
  "dev": "nodemon server.js"
}
```
(Might try Vite next time? But manual Express configuration offers more transparency)