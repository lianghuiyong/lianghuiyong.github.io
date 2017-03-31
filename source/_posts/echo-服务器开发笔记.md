---
title: echo 服务器开发笔记
date: 2017-03-31 09:36:24
tags: [服务器开发]
---

## 1、解决Http x509 未认证的验证问题

```
func newClient() *http.Client {
	tr := &http.Transport{
		TLSClientConfig: &tls.Config{InsecureSkipVerify: true},
	}
	return &http.Client{Transport: tr}
}
```