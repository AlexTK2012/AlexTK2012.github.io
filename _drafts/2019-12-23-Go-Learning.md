---
layout:     post         
title:      "Go语言学习"
date:       2019-12-23 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "mg/post-bg/home-bg.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Go
---

[Golang 新手可能会踩的 50 个坑](https://segmentfault.com/a/1190000013739000)

Go先配置GOPATH 和 GOROOT

Go下载项目

* 有go.mod 就执行 **go mod tidy**
* 没有就 go get -d -v ./..

```sh
#可以将项目下载到 $GOPATH 目录下
#公司环境里，有些包可能需要走proxy，或者https认证先(或者git alis 映射到ssh协议)
go get domain.com/path/to/sub/directory
```
