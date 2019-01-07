---
title: "Expressの基本処理"
date: 2018-11-11T10:52:57+09:00
weight: 7
#draft: true
---

## Quick Start

```bash
npm install --save body-parser
```

```javascript
import express from 'express';

const app = express();

app.get('/', (req, res) => {
  res.send('Hello Node.js-Express!');
});

app.listen(4000, () => {
  console.log('Start server at 4000.');
});
```

## Node Test Framework

{{% notice info %}}
TL;DR; Use Jest for unit and integration tests and TestCafe for UI tests.
{{% /notice %}}

- Jasmine
- Mocha (Mocha, Chai, Sinon)
- JTest (React Test Framework, on the top of Jasmine)

#### Reference

- [Medium: An Overview of JavaScript Testing in 2018](<https://medium.com/welldone-software/an-overview-of-javascript-testing-in-2018-f68950900bc3>)