---
layout: post
title: git稀疏检出 特定目录
abbrlink: 9f4a85a5eec448ab8f81a5aaddf4ecef
tags:
  - git
categories:
  - IT
  - 代码版本管理
  - Git
date: 1631341383600
updated: 1758161740970
---

启用`core.sparsecheckout`后
修改该文件：
`.git/info/sparse-checkout`

文件名直接写，目录以/结尾
前面加!用来排除

```bash
#!/usr/bin/env bash
git_url=${1}
proj_directory=${2}

if [[ ! -n ${git_url} ]] ;then
	echo "未指定远端git仓库地址"
	exit 1
fi

if [[ ! -n ${proj_directory} ]] ;then
	proj_directory=${git_url##*/}
	proj_directory=${proj_directory%.*}
fi

echo "初始化项目目录"
git init ${proj_directory}
cd ${proj_directory}

echo "设置远端git仓库地址"
git remote add origin ${git_url}

echo "启用稀疏检出"
git config core.sparsecheckout true
echo "/*" >> .git/info/sparse-checkout
echo "!doc/" >> .git/info/sparse-checkout

echo "拉取代码"
git pull origin master

```
