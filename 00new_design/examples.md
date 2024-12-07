我很乐意为您翻译这个项目概述文档。我会保持专业术语的准确性，同时确保中文表达通顺自然。

# 项目概述

您的目标是构建一个 Next.js 应用程序，该程序允许用户上传 PDF 文件，并使用 OpenAI 的结构化输出功能从 PDF 文件中提取信息并转换为 Excel 文件。
您将使用 NextJS 14、shadcn、tailwind 和 Lucid icon 进行开发。

# 核心功能

## 1. 文件上传与模式定义

- 用户应能够上传一个或多个 PDF 文件
- 用户应能够定义他们想要提取的数据点：
  - 单个字段（单值提取）
  - 分组（具有一致结构的对象数组）
  - 对于每个分组，用户可以在其中定义多个字段，甚至其他分组
- 应该有一个"开始提取"按钮
- 设置默认模式以展示工作原理：
  - company：公司名称
  - address：公司地址
  - total sum：我们购买的总金额
  - items（分组）：
    - item：商品名称
    - unit price：商品单价
    - quantity：购买数量
    - sum：购买总金额
- 服务器端文件处理

## 2. 文本提取

- 使用 LlamaParser 进行 PDF 文本提取（服务器端）
- 对于每个文件，合并所有文档片段以获得所有文档的完整文本，而不仅仅是第一个文档 documents[0]
- LlamaParser 文本提取应该在用户上传文件到界面后立即进行，而不是等待按钮点击
- 严格遵循 ## 1. LlamaParser 文档作为代码实现示例
- 每个文件上传后，应在页面上显示为一个项目，显示文件名并带有一个按钮可点击预览提取的完整文本
- 用户可以继续向列表添加新文件，之前上传的文件应保持显示
- 仅服务器端处理

## 3. 数据处理

- 点击"开始提取"后，应将数据发送给 OpenAI 处理所有文件
- 使用 OpenAI 结构化输出进行信息提取
- 严格遵循 ## 2. OpenAI 文档作为代码实现示例

## 4. 文件下载

- 将多个 PDF 处理的数据合并到一个 Excel 文件中
- 当存在嵌套结构如 {'company': xxx, 'items': [{'item': xxx, 'quantity': xxx, 'sum': xxx}]} 时，生成 Excel 文件时应该将其展平
- 实现适当的错误处理和类型安全
- 启用 Excel 文件下载
- 实现临时文件清理

# 重要实施说明

## 0. 添加日志

- 始终在代码中添加服务器端日志，以便我们可以调试潜在问题

## 1. 项目设置

- 所有新组件都应放在根目录的 /components 中（不是在 app 文件夹中），除非另有说明，命名方式为 example-component.tsx
- 所有新页面都放在 /app 中
- 使用 Next.js 14 app 路由
- 所有数据获取都应在服务器组件中完成，并通过 props 向下传递数据
- 客户端组件（useState、hooks 等）需要在文件顶部设置 'use client'

## 2. 服务器端 API 调用：

- 所有与外部 API 的交互（如 Reddit、OpenAI）都应在服务器端执行
- 在 'pages/api' 目录中为每个外部 API 交互创建专用的 API 路由
- 客户端组件应通过这些 API 路由获取数据，而不是直接从外部 API 获取

## 3. 环境变量：

- 将所有敏感信息（API 密钥、凭据）存储在环境变量中
- 在本地开发中使用 `.env.local` 文件，并确保将其列入 `.gitignore`
- 在生产环境中，在部署平台（如 Vercel）中设置环境变量
- 仅在服务器端代码或 API 路由中访问环境变量

## 4. 错误处理和日志：

- 在客户端组件和服务器端 API 路由中实现全面的错误处理
- 在服务器端记录错误以便调试
- 在客户端显示用户友好的错误消息

## 5. 类型安全：

- 为所有数据结构使用 TypeScript 接口，特别是 API 响应
- 避免使用 'any' 类型；相反，为所有变量和函数参数定义适当的类型

## 6. API 客户端初始化：

- 仅在服务器端代码中初始化 API 客户端（如用于 Reddit 的 Snowwrap、OpenAI）
- 实现检查以确保在使用前正确初始化 API 客户端

## 7. 组件中的数据获取：

- 在客户端组件中使用 React hooks（如 `useEffect`）进行数据获取
- 为所有数据获取操作实现加载状态和错误处理

## 8. Next.js 配置：

- 使用 `next.config.mjs` 进行特定环境的配置
- 使用 `next.config.mjs` 中的 `env` 属性使环境变量对应用程序可用

## 9. CORS 和 API 路由：

- 使用 Next.js API 路由来避免与外部 API 交互时的 CORS 问题
- 在 API 路由中实现适当的请求验证

## 10. 组件结构：

- 分离客户端和服务器组件的关注点
- 使用服务器组件进行初始数据获取，并通过 props 将数据传递给客户端组件



# Project overview

Your goal is to build a next.js app that allows users to upload PDF files, and use OpenAI structured output feature to extract information from the PDF file and convert to excel file.
You will be using NextJS 14, shadcn, tailwind, Lucid icon

# Core functionalities

## 1. File Upload & Schema Definition
- Users should be able to upload one or more PDF files
- Users should be able to define data points they want to extract:
  - Individual fields (single value extractions)
  - Groups (array of objects with consistent structure)
  - For each group, users can define multiple fields inside, or even other groups
- There should be a button 'Start extraction'
- Set default schema to showcase how does this work
  - company: name of company
  - address: address of company
  - total sum: total amount we purchased
  - items (group):
    - item: name of item
    - unit price: unit price of item
    - quantity: quantity we purchased
    - sum: total amount we purchased
- Server-side file processing

## 2. Text Extraction
- Use LlamaParser for PDF text extraction (server-side)
- For each file, combine all document chunks for complete text of all documents, not just the first one documents[0]
- The llamaparser text extraction should happen immediately after user upload files to UI, and not wait for a button click
- Strictly following ## 1. LlamaParser Documentation as code implementation example
- After each file uploaded, it should be displayed as an item on the page, displaying the file name with a button to click to preview the full text extracted
- User can keep adding new files to the list, previously uploaded files should be displayed
- Server-side processing only

## 3. Data Processing
- After clicking on 'Start Extraction', the data should be sent to OpenAI for processing across all files
- Use OpenAI structured output for information extraction
- Strictly following ## 2. OpenAI Documentation as code implementation example

## 4. File Download
- Combine data processed from multiple PDFs into one excel file
- When there are nested structure like {'company': xxx, 'items': [{'item': xxx, 'quantity': xxx, 'sum': xxx}]}, it should be flattened when generating the excel file
- Implement proper error handling and type safety
- Enable excel file download
- Implement temporary file cleanup

## Docs



## Important Notes

Here's the content of both screenshots in markdown format:

# Important Implementation Notes

## 0. Adding logs
- Always add server side logs to your code so we can debug any potential issues

## 1. Project setup
- All new components should go in /components at the root (not in the app folder) and be named like example-component.tsx unless otherwise specified
- All new pages go in /app
- Use the Next.js 14 app router
- All data fetching should be done in a server component and pass the data down as props
- Client components (useState, hooks, etc) require that 'use client' is set at the top of the file

## 2. Server-Side API Calls:
- All interactions with external APIs (e.g., Reddit, OpenAI) should be performed server-side.
- Create dedicated API routes in the 'pages/api' directory for each external API interaction.
- Client-side components should fetch data through these API routes, not directly from external APIs.

## 3. Environment Variables:
- Store all sensitive information (API keys, credentials) in environment variables.
- Use a `.env.local` file for local development and ensure it's listed in `.gitignore`.
- For production, set environment variables in the deployment platform (e.g., Vercel).
- Access environment variables only in server-side code or API routes.

## 4. Error Handling and Logging:
- Implement comprehensive error handling in both client-side components and server-side API routes.
- Log errors on the server-side for debugging purposes.
- Display user-friendly error messages on the client-side.

## 5. Type Safety:
- Use TypeScript interfaces for all data structures, especially API responses.
- Avoid using 'any' type; instead, define proper types for all variables and function parameters.

## 6. API Client Initialization:
- Initialize API clients (e.g., Snowwrap for Reddit, OpenAI) in server-side code only.
- Implement checks to ensure API clients are properly initialized before use.

## 7. Data Fetching in Components:
- Use React hooks (e.g., `useEffect`) for data fetching in client-side components.
- Implement loading states and error handling for all data fetching operations.

## 8. Next.js Configuration:
- Utilize `next.config.mjs` for environment-specific configurations.
- Use the `env` property in `next.config.mjs` to make environment variables available to the application.

## 9. CORS and API Routes:
- Use Next.js API routes to avoid CORS issues when interacting with external APIs.
- Implement proper request validation in API routes.

## 10. Component Structure:
- Separate concerns between client and server components.
- Use server components for initial data fetching and pass data as props to client components.



