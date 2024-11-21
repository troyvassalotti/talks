---
title: Quick and Simple Tooling with Gulp
date: 2022-02-18
tags: ccjs
updated: 2024-11-20
---

# Quick and Simple Tooling with Gulp

---

## Why

<img src="file:///home/troy/Documents/obsidian-vault/30-39 Programming/32 charmCityJs/32.03 Gulp/attachments/Pasted image 20220219131149.png" alt="" width="40%">

- You have assets you want to automate processing of (SCSS -> CSS, TS -> JS, PNG -> WebP, etc.)

---

## My Situation

- Our team mainly serves external files from our S3 buckets. We had a git repo, but it was more like a backup location than a working location.
- That repo had code pretending to be something it wasn't (minified code was _uncompressed_ and labelled as the uncompressed version)
- Gulp was installed, but not actively used and those who knew how it was setup were gone.

---

## Getting Started

1. Make sure you have Node.js & npm installed

```shell
node --version
v.16.14.0

npm --version
8.3.1
```

1. Install the gulp command line utility globally

```shell
npm install -g gulp-cli
gulp --version
```

---

## What Will We Do

1. Get our project setup with `npm`
2. Install our packages
3. Create our assets
4. Gulp them up

---

## Create Your Project

```shell
cd ~/Projects
mkdir ccjs-gulp
cd ccjs-gulp
```

---

## Live Coding Time

Wish me luck.
