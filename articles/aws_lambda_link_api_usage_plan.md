---
title: 'API Gatewayの{"message":"Forbidden"}'
emoji: "⛳"
type: "tech"
topics: ["aws", "api gateway"]
published: true
---


Lambda の「トリガーを追加」から、API Gatewayを追加しても、そのままでは使えない。

<img width="531" alt="スクリーンショット 2021-12-21 3 09 45" src="https://user-images.githubusercontent.com/37844673/146813072-91ee7e54-9744-4998-aafd-64a7dc064c75.png">


↓によると、API Usage planをリンクしないといけなくって、

https://stackoverflow.com/questions/39061041/using-an-api-key-in-amazon-api-gateway

ここの部分

<img width="608" alt="スクリーンショット 2021-12-21 3 11 09" src="https://user-images.githubusercontent.com/37844673/146813242-e2bdf098-0018-420a-9cd9-21ee50f9e34a.png">
