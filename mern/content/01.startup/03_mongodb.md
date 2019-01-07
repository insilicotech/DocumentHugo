---
title: "MongoDB"
date: 2018-11-11T12:56:59+09:00
#draft: true
weight: 8
---

## MongoDBの準備

### MongoDBをインストールする

```bash
brew update && brew install mongodb

# Mongo-Expressのインストール
npm i -g mongo-express
```

### MongoDBを使う

```bash
# 1. MongoDB Serverを起動する(手動)
mongod --directoryperdb --dbpath /IST/Data/mongodb --logpath /IST/log/mongodb/mongo.log --logappend

# 2. Mongoクライアントを使う
mongo --host localhost:27017
show databases
```

### Reference

- [Robo 3T(former RoboMongo) Download(Native and cross-platform MongoDB manager)](<https://robomongo.org/download>)

## MongoDBの基本知識

|  SQL        |      NoSQL      |
| ------------- | ----------- |
| Table         | Collection |
| Row/Record    | Document   |
| Column        | Field   |

#### Reference

DefintionName
: A Defintion explain

-   Udemy_The.Complete.Node.Developer.Course(2e)
