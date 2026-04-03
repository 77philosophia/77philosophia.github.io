---
title: backend
categories:
  - 专业笔记
date: 2024-05-23 14:00:15
tags:
---



# Week 1

#### 1. Node js

Node js是javascript在服务器端的编程框架， express是其开发框架。

server大概有三种：database server / web server(http) / application server

js的特点：Event-driven, 异步asynchronous, 非阻塞，[js了解](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9SZWFkaW5ncy9TZXJ2ZXJTaWRlSmF2YXNjcmlwdC5tZCIsInRvb2xfdHlwZSI6Imluc3RydWN0aW9uYWwtbGFiIiwiYWRtaW4iOmZhbHNlLCJpYXQiOjE3MTE0MjY1MTZ9.6yLbDHSnA7Ffyp8kq9u1jq8zNAs1QiwSi8fIG0quUxg)



#####  js module

![image-20240523143047893](backend/image-20240523143047893.png)

![image-20240523143135653](backend/image-20240523143135653.png)

![image-20240523143215979](backend/image-20240523143215979.png)



![image-20240523143324541](backend/image-20240523143324541.png)

![image-20240523143354049](backend/image-20240523143354049.png)

![image-20240523144020865](backend/image-20240523144020865.png)

![image-20240523144848936](backend/image-20240523144848936.png)

![image-20240523145015691](backend/image-20240523145015691.png)

![image-20240523145042861](backend/image-20240523145042861.png)

[高级js模块](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9SZWFkaW5ncy9BZHZhbmNlZF9Ob2RlLmpzX21vZHVsZXMubWQiLCJ0b29sX3R5cGUiOiJpbnN0cnVjdGlvbmFsLWxhYiIsImFkbWluIjpmYWxzZSwiaWF0IjoxNzExNDI2NTUzfQ.oUQg6uIJbbEgp0GByD36iHMr_gMmReyJYVykaSrHGbA)

![image-20240523145912229](backend/image-20240523145912229.png)

[lab:使用nodejs创建服务端程序](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9sYWJzJTJGTW9kdWxlMV9JbnRyb2R1Y3Rpb25Ub1NlcnZlclNpZGVKYXZhU2NyaXB0JTJGSGFuZHNPbl9MYWJfRmlyc3RTZXJ2ZXJTaWRlU2NyaXB0Lm1kIiwidG9vbF90eXBlIjoidGhlaWEiLCJhZG1pbiI6ZmFsc2UsImlhdCI6MTcxMTQyNjUyNH0.4cNsJQ7n8QzVT8x1BO0QhMW5jb7Kp_aXW3VILCc2h9Y)

![image-20240523152832502](backend/image-20240523152832502.png)



[一些概念](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9HbG9zc2FyeS8yMDAzODkuMTFfTTFfR2xvc3NhcnkubWQiLCJ0b29sX3R5cGUiOiJpbnN0cnVjdGlvbmFsLWxhYiIsImFkbWluIjpmYWxzZSwiaWF0IjoxNzExNDI2NTQ0fQ.TuD3KX8yEEewo2CIDv2AyjGmWI1EMdClt8Tdw1m5MKY)

[服务器端js简介](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9DaGVhdHNoZWV0cy8yMDAzODkuMjVfTTFfQ2hlYXRTaGVldC5tZCIsInRvb2xfdHlwZSI6Imluc3RydWN0aW9uYWwtbGFiIiwiYWRtaW4iOmZhbHNlLCJpYXQiOjE3MTE0MjY1MzV9.4M92HAb93JO5_DdTXT_jL2vJa-pfHrJ3Cxw2C_HASVE)



# Week 2



nodejs通过非阻塞的方式调用所有io

![image-20240523154546786](backend/image-20240523154546786.png)

![image-20240523154705545](backend/image-20240523154705545.png)

![image-20240523185448246](backend/image-20240523185448246.png)

[async/await](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9SZWFkaW5ncy9JbnRyb2R1Y3Rpb25Ub0FzeW5jQXdhaXQubWQiLCJ0b29sX3R5cGUiOiJpbnN0cnVjdGlvbmFsLWxhYiIsImFkbWluIjpmYWxzZSwiaWF0IjoxNzExNDI2NTU3fQ.QLu1ThZK6P3pv6Q24iyROVj8RZZnSPyZzOIx3e7Kuz0)

[异步回调编程](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9sYWJzJTJGTW9kdWxlMl9Bc3luY19DYWxsYmFjayUyRkhhbmRzT25fTGFiX0FzeW5jX0NhbGxiYWNrLm1kIiwidG9vbF90eXBlIjoidGhlaWEiLCJhZG1pbiI6ZmFsc2UsImlhdCI6MTcxMTQyNjUzMX0.qV1jLSpoScLWvGOZKXv95pbhkyzEOctJ9yOy5CscDQ4)

![image-20240524112320066](backend/image-20240524112320066.png)

[术语表 - 使用回调编程的异步 I/O]()

[Asynchronous I/O with Callback Program](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9DaGVhdHNoZWV0cy8yMDAzODkuMjVfTTJfQ2hlYXRTaGVldC5tZCIsInRvb2xfdHlwZSI6Imluc3RydWN0aW9uYWwtbGFiIiwiYWRtaW4iOmZhbHNlLCJpYXQiOjE3MTE0MjY1Mzh9.JPvmEnhXrPQHLGLyZDW-7kzx2espu7krfqxdFJp9Ao0)



# Express网络框架

node js不是一个运行框架，是一个runtime environment用于在server上面运行js code

![image-20240524115011622](backend/image-20240524115011622.png)

![image-20240524115044107](backend/image-20240524115044107.png)

![image-20240524115606544](backend/image-20240524115606544.png)



#### 中间件和路由器

本文将讨论*中间件* 和*路由*这两个术语。

中间件是位于应用程序、数据库或服务之间的软件，允许这些不同的技术进行通信。它在分布式系统中为终端用户创建无缝交互。

Express 是一个消息传递框架，用于处理路由和编写中间件。应用程序的前端使用 Express 来促进后端组件之间的通信，而前端和后端服务无需使用相同的语言。前端与中间件通信，而不是直接与后端通信。

像 Express 这样的消息框架通常包含 JSON、REST API 和网络服务。旧的消息框架可能包含可扩展标记语言（XML）和简单对象访问协议（SOAP），而不是 JSON 和 REST API。消息传递框架提供了一种处理不同应用间数据传输的标准化方法。

网络服务器是连接网站和数据库的中间件的一个例子。网络服务器处理业务逻辑，并根据请求从数据库路由数据。*路由* 是将 GET、POST 或 DELETE 等 HTTP 请求与 URL 以及处理该 URL 的函数相关联的代码部分。路由在网络开发中用于根据浏览器 URL 确定的规则分割应用程序的用户界面。

路由器功能统称为 "中间件"。中间件负责响应 HTTP 请求或调用中间件链中的其他函数。Express 通过 Router 类处理路由器函数，如 Router.get()。顾名思义，Router.get() 负责处理 HTTP GET 请求。其他路由器函数包括 Router.post()、Router.put() 和 Router.delete()，使用方法基本相同。这些方法需要两个参数，一个 URL 路径和一个回调函数。

除了路由之外，中间件还负责通过加密和解密数据来提供服务间的安全连接，通过将流量分配到不同的服务器来管理应用程序负载，以及在数据返回客户端之前对数据进行排序或过滤。



[身份验证和授权](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9SZWFkaW5ncy9JbnRyb2R1Y3Rpb24tdG8tQXV0aGVudGljYXRpb24ubWQiLCJ0b29sX3R5cGUiOiJpbnN0cnVjdGlvbmFsLWxhYiIsImFkbWluIjpmYWxzZSwiaWF0IjoxNzExNDI2NTU1fQ.iKf3HQ4FD9ESfd0H1cH8R1eONRumBzDQAQYVyD3SO34)

[http method/rest api](https://d3c33hcgiwev3.cloudfront.net/L1T_uyeHSHGyROFLyqZRxQ_2ee09d6aa6ba4736920c2f6a31faa6f1_Writing-RESTful-APIs-Reading.docx.pdf?Expires=1716681600&Signature=IJLmbTFs~gRZ3Ij9yrXrNVeNbu5kADjvREy0SkQt7eovyMZNnOX6YaZFa7Cs2fOVg4Fg4JOsSBxmsgho3a~kMxWsDVQGpDO79Ai2NjvK1Jv5~pe0z7FlTvajvO62pgLJFpBaegWY12EM5mjzVwzMHDILZrSNF7luz~2R3gb8-3s_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)

[lab:crud with nodejs](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9sYWJzJTJGTW9kdWxlM19FeHByZXNzSlMlMkZIYW5kcy1vbl9MYWJfQ1JVRC5tZCIsInRvb2xfdHlwZSI6InRoZWlhIiwiYWRtaW4iOmZhbHNlLCJpYXQiOjE3MTE0MjY1MjZ9.30Vfc7qrnD9jo_XAW4oiq9zJ1ax_0swsPwmRIL34JLk)



![image-20240524174736995](backend/image-20240524174736995.png)

![image-20240524174805639](backend/image-20240524174805639.png)

[express web术语](https://www.coursera.org/learn/developing-backend-apps-with-nodejs-and-express/ungradedWidget/IO1Oa/glossary-express-web-application-framework)

test-project/
   node_modules/
   config/
     db.js           //Database connection and configuration
     credentials.js  //Passwords/API keys for external services used by your app
   models/            //For mongoose schemas
      items.js
      prices.js
   routes/           //All routes for different entities in different files
      items.js
      prices.js
   app.js
   routes.js         //Require all routes in this and then require this file in
   package.json



[practice lab](https://author-ide.skills.network/render?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZF9pbnN0cnVjdGlvbnNfdXJsIjoiaHR0cHM6Ly9jZi1jb3Vyc2VzLWRhdGEuczMudXMuY2xvdWQtb2JqZWN0LXN0b3JhZ2UuYXBwZG9tYWluLmNsb3VkL0lCTURldmVsb3BlclNraWxsc05ldHdvcmstQ0QwMjIwRU4tU2tpbGxzTmV0d29yay9sYWJzJTJGUHJhY3RpY2VQcm9qZWN0X0ZyaWVuZHNMaXN0X1dpdGhBdXRoJTJGaW5zdHJ1Y3Rpb25zLm1kIiwidG9vbF90eXBlIjoidGhlaWEiLCJhZG1pbiI6ZmFsc2UsImlhdCI6MTcxMTQyNjUyM30.0Rshfz3KAokqUCOpGERC0zmSL26a7aKKCtJLaZIUsKc)
