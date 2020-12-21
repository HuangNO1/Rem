---
title: "部署 SpringBoot 與 Vue 前後端完全分離 Web 項目"
date: 2020-12-21T16:49:28+08:00
# 副標題
subtitle: ""
# 上次修改的時間
lastmod: 2020-12-21T16:49:26+08:00
draft: false
description: ""
license: ""
tags: ["Vue", "Vue-cli", "Web", "npm", "SpringBoot", "Linux", "Java", "JDK", "Maven", "IntelliJ", "IDEA", "Jar", "Nginx"]
categories: ["Linux"]
featuredImage: "/featuredImage/compressed/deploy_springboot_vue_web.png"
---

## 前言

由於這學期我選修了軟件測試的課程，我們以十人小組為單位，對我在暑假實訓所參預的項目進行 Web 測試，過程中我將那個項目部署到了阿里雲的雲服務器上。這篇文章我紀錄一下我部署 Web 的過程。

## 將前後端打包

### 前端打包

執行 `npm run build` 就可以將