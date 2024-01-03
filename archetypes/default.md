---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date | dateFormat "2006-01-01"}}
draft: true

ShowToc: true           # 显示toc
TocOpen: true           # 自动展开目录
hideMeta: false         # 是否隐藏文章的元信息，如发布日期、作者等
summary: ""
lastMod: {{ .Date | dateFormat "2006-01-01"}}
cover:
    hidden: true
    # image: "post/golang-001.jpeg"
weight: 30
categories: []

tags: []
---
