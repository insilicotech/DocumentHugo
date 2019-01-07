---
title: "Node Redis"
date: 2018-11-14T22:48:35+09:00
#draft: true
weight: 9
---

## 環境構築

### Redis環境構築

```bash
brew update && brew install redis
brew service start redis
redis-cli pin
# -> PONG
```

### NodeJS環境構築

```bash
npm install --save redis
```

テストソース

```javascript
import redis from 'redis';

const redisURL = 'redis://localhost:6379';
const client = redis.createClient(reidsURL);

client.set('hi', 'IST');
client.get('hi', (err, value) = {
  console.log(value);
})
```
