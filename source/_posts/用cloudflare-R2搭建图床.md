---
title: 用cloudflare R2搭建图床
date: 2024-10-24 09:59:48
author: Evan
categories: 笔记
tags:
---

# Cloudflare R2存储桶创建与Worker实现教程

## 1. 创建Cloudflare R2存储桶

### 第一步：登录Cloudflare账户
访问 [Cloudflare官网](https://www.cloudflare.com/)，登录到你的Cloudflare账户。如果你还没有账户，请先创建一个。

### 第二步：导航到R2存储
1. 登录后，在Cloudflare Dashboard的左侧导航栏中，找到 **R2** 服务选项。
2. 如果你未找到，可以在顶部的搜索框输入 "R2" 搜索并选择。

### 第三步：创建R2存储桶
1. 点击 **Create Bucket** 按钮。
2. 为存储桶输入一个唯一名称（存储桶名必须全局唯一）。
3. 点击 **Create Bucket** 完成存储桶的创建。

### 第四步：获取存储桶的Access Keys
1. 在R2存储桶页面，点击 **Access Keys**。
2. 点击 **Create Access Key**，系统将生成 `Access Key` 和 `Secret Key`，将这些信息保存下来，它们将在Worker中使用。



---

## 2. 创建Cloudflare Worker

### 第一步：进入Worker管理页面
1. 在Cloudflare Dashboard左侧导航栏中，找到并点击 **Workers**。
2. 点击 **Create a Service** 按钮。
3. 输入服务的名称，然后选择 **HTTP handler** 作为Worker的类型，点击 **Create Service**。

### 第二步：配置R2绑定
1. 在Worker服务页面，点击 **Resources**。
2. 在 **R2 Bucket** 选项中，选择你之前创建的R2存储桶。
3. 为绑定的存储桶选择一个变量名称，比如 `MY_BUCKET`。

### 第三步：编写Worker代码
你可以通过Cloudflare Dashboard直接编辑Worker的代码。在Worker的编辑器中，编写以下代码：

```javascript
export default {
  async fetch(request, env) {
    const url = new URL(request.url);

    // 上传文件
    if (url.pathname === "/upload" && request.method === "POST") {
      const formData = await request.formData();
      const file = formData.get("file");

      if (!file) {
        return new Response("No file uploaded", { status: 400 });
      }

      const fileName = file.name;
      const bucket = env.MY_BUCKET;
      await bucket.put(fileName, file.stream());

      return new Response(`File ${fileName} uploaded successfully.`);
    }

    // 读取文件
    if (url.pathname === "/download" && request.method === "GET") {
      const fileName = url.searchParams.get("file");

      if (!fileName) {
        return new Response("No file specified", { status: 400 });
      }

      const bucket = env.MY_BUCKET;
      const file = await bucket.get(fileName);

      if (!file) {
        return new Response("File not found", { status: 404 });
      }

      return new Response(file.body, {
        headers: {
          "Content-Type": "application/octet-stream",
          "Content-Disposition": `attachment; filename="${fileName}"`,
        },
      });
    }

    return new Response("Not found", { status: 404 });
  },
};
```

### 第四步：保存并部署Worker

1. 编辑完成后，点击页面右上角的 **Save and Deploy** 按钮保存代码并将其部署到你的Worker域名。
2. 部署完成后，你将会获得一个Worker的URL，可以用于测试和访问



## 3. 使用案例

### 上传文件：

使用 `POST` 请求向 `/upload` 端点上传文件：

```bash
curl -X POST https://<你的worker域名>/upload -F "file=@<本地文件路径>"
```

### 下载文件：

通过 `GET` 请求从R2存储桶下载文件：

```bash
curl https://<你的worker域名>/download?file=<文件名>
```
