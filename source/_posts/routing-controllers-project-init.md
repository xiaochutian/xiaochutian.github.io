---
title: Routing Controllers Framework 项目初始化(使用express.js)
date: 2018-05-29 20:20:20
categories:
 - 技术博客
 - 服务器
 - Node.js
tags:
 - routing-controllers
---

# 简介

> Create structured, declarative and beautifully organized class-based controllers with heavy decorators usage in Express / Koa using TypeScript and Routing Controllers Framework.

一言以蔽之：可以像开发 `Java Web` 项目一样，愉快的使用注解进行开发。

<!-- more -->

# 开发环境

* 语言环境：[node](https://nodejs.org/zh-cn/)
* 包管理工具：[cnpm](https://npm.taobao.org/)
* 集成开发环境：[idea](https://www.jetbrains.com/idea/)

# 基本步骤

1. 创建项目目录结构
2. 根据[【官方文档】安装](https://github.com/typestack/routing-controllers#installation) 初始化项目
3. 运行项目并测试
4. 初始化git配置并提交（非必须）

# 详细步骤

## 创建项目目录结构

```bash
cd ~
mkdir routing-controllers-learn
cd routing-controllers-learn
mkdir config
mkdir controller
mkdir service
```

创建后的目录结构如下：

```bash
➜  routing-controllers-learn pwd
/Users/xiaochutian/routing-controllers-learn
➜  routing-controllers-learn ll
total 0
drwxr-xr-x 2 xiaochutian 64 May 29 23:50 config
drwxr-xr-x 2 xiaochutian 64 May 29 23:50 controller
drwxr-xr-x 2 xiaochutian 64 May 29 23:50 service
```

## 初始化Routing Controllers项目

### 安装项目依赖

```bash
cd ~/routing-controllers-learn
cnpm install routing-controllers --save
cnpm install reflect-metadata --save
cnpm install express body-parser multer --save
cnpm install @types/express @types/body-parser @types/multer --save
```

安装后的 `package.json` 如下：

```json
{
  "dependencies": {
    "@types/body-parser": "^1.17.0",
    "@types/express": "^4.11.1",
    "@types/multer": "^1.3.6",
    "body-parser": "^1.18.3",
    "express": "^4.16.3",
    "multer": "^1.3.0",
    "reflect-metadata": "^0.1.12",
    "routing-controllers": "^0.7.7"
  }
}
```

### 配置tsconfig.json

```bash
cd ~/routing-controllers-learn
vim tsconfig.json
```

`tsconfig.json` 文件内容如下：

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es7",
    "sourceMap": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "lib": [
      "dom",
      "es7"
    ]
  },
  "exclude": [
    "node_modules"
  ]
}

```

### 创建UserController

```bash
cd ~/routing-controllers-learn/controller
vim UserController.ts
```

`UserController.ts` 文件内容如下：

```typescript
import {Controller, Param, Body, Get, Post, Put, Delete} from "routing-controllers";

@Controller()
export class UserController {

    @Get("/users")
    getAll() {
        return "This action returns all users";
    }

    @Get("/users/:id")
    getOne(@Param("id") id: number) {
        return "This action returns user #" + id;
    }

    @Post("/users")
    post(@Body() user: any) {
        return "Saving user...";
    }

    @Put("/users/:id")
    put(@Param("id") id: number, @Body() user: any) {
        return "Updating a user...";
    }

    @Delete("/users/:id")
    remove(@Param("id") id: number) {
        return "Removing user...";
    }

}
```

### 创建App入口

```bash
cd ~/routing-controllers-learn
vim app.ts
```

`app.ts` 文件内容如下：

```typescript
import "reflect-metadata"; // this shim is required
import {createExpressServer} from "routing-controllers";
import {UserController} from "./controller/UserController";

// creates express app, registers all controller routes and returns you express app instance
const app = createExpressServer({
    controllers: [UserController] // we specify controllers we want to use
});

// run express application on port 3000
app.listen(3000);
```

### IDEA中typescript生成javascript

用 `IDEA` 打开项目，打开 `app.ts` 文件。根据提示，把 `IDEA` 中，`typescript` 编译生成 `javascript` 的设置打开

![ts2js](/images/post-routing-controllers-project-init/ts2js.jpg)

> 之后，需要研究一下，在服务器上把typescript编译成javascript的配置

## 运行项目并测试

### 安装nodemon

安装 `nodemon` 之后，本地文件变更会自动编译部署，调试起来很方便。    
（这个比起 `Java Web` 爽太多了）

```bash
cnpm install -g nodemon
```

### 本地运行程序

```bash
cd ~/routing-controllers-learn
nodemon app
```

### 测试

浏览器打开 `http://localhost:3000/users` ，则返回 `This action returns all users`    
浏览器打开 `http://localhost:3000/users/1` ，则返回 `This action returns user #1`

## 初始化git配置并提交

```bash
cd ~/routing-controllers-learn
echo ".idea" > .gitignore
git init
git add .
git commit -m "routing-controllers项目初始化提交"
```

# 参考
[【官方文档】安装](https://github.com/typestack/routing-controllers#installation)    
[【淘宝】淘宝NPM镜像](https://npm.taobao.org/)    
