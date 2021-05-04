---
title: "Python 爬蟲實戰教學 - Github 倉庫特徵爬取"
date: 2021-05-04T08:03:28+08:00
# 副標題
subtitle: ""
# 上次修改的時間
lastmod: 2021-05-04T08:03:28+08:00
draft: false
description: ""
license: ""
tags: ["Python", "Spider", "Github", "Repository"]
categories: ["Python"]
featuredImage: "/featuredImage/compressed/python_spider_experience_instruct_github_repos.jpg"
---

## 前言

因為這期學期上半我選了機器學習與數據挖掘，我在課程小組項目中負責了爬蟲這個部份，我會在這篇文章紀錄一下我的過程，並教導大家如何實戰 Python 爬蟲。

## 項目說明

我們這個小組項目的爬取需求是爬取 Github 項目的各個特徵，這篇文章會分兩個部份，**第一個部份是使用 `BeautifulSoup` 爬取原生的 HTML 頁面**，**第二部份是使用 Github API**，其中第二部份包含了每個倉庫的 Commits、pull_request月變化量。

## 開發前準備

因為我們需要爬取 Github 的網頁和使用 Github API，我們需要申請 Personal token，避免我們的請求達到 Rate Limit，Github 有限制一分鐘內的請求次數。




