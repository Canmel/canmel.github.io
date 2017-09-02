---
layout: post
title: "spring cloud RestTemplate 几种的请求"
description: ""
category: 
tags: []
---
{% include JB/setup %}

---

### GET请求

| 方法名  | 返回　| 参数 | 说明 |
| --- | --- | --- | --- |
|`getForEntity`| org.springframework.http.ResponseEntity |`String` url, `Class` responseType, `Object`... urlVariables| 第三个参数是一个数组，顺序对应`url`占位符顺序 |
|`getForEntity`| org.springframework.http.ResponseEntity |`String` url, `Class` responseType, Map uriVariables| url定义：　http://xxx.xxx?name={name}, map参数必须有一个key为`name`的键值对 |
|`getForEntity`| org.springframework.http.ResponseEntity |`URI` url, `Class` responseType | 使用一个统一资源标识符 |
|`getForObject`| 自定义　`responseType` |`String` url, `Class` responseType, `Object`... uriVariables| `uriVariables`为`url`中占位符对应的参数 |
|`getForObject`| 自定义　`responseType` |`String` url, `Class` responseType, `Map` uriVariables| `uriVariables`为`url`中占位符对应的参数,名称需要与`map`的`key`对应起来 |
|`getForObject`| 自定义　`responseType` |`String` url, `Class` responseType | 使用一个统一资源标识符 |

---

### POST请求

| 方法名  | 返回　| 参数 | 说明 |
| --- | --- | --- | --- |
|`postForEntity`| org.springframework.http.ResponseEntity |`String` url, `Class` responseType, `Object`... urlVariables| 第三个参数是一个数组，顺序对应`url`占位符顺序 |
|`postForEntity`| org.springframework.http.ResponseEntity |`String` url, `Object` request, `Class` responseType, `Map` uriVariables| url定义：　http://xxx.xxx?name={name}, `map`参数必须有一个key为`name`的键值对 |
|`postForEntity`| org.springframework.http.ResponseEntity |`URI` url, `Object` request, `Class` responseType| 使用一个统一资源标识符 |
|`postForObject`| 自定义　`responseType` |`String` url, `Object` request, `Class` responseType, `Object`... uriVariables| 第三个参数是一个数组，顺序对应`url`占位符顺序 |
|`postForObject`| 自定义　`responseType` |`String` url, `Object` request, `Class` responseType, `Map` uriVariables| url定义：　http://xxx.xxx?name={name}, `map`参数必须有一个key为`name`的键值对 |
|`postForObject`| 自定义　`responseType` |`URI` url, `Object` request, `Class` responseType| 使用一个统一资源标识符 |
|`postForLocation`|`URI`| `String` url, `Object` request, `Object`... uriVariables | 第三个参数是一个数组，顺序对应`url`占位符顺序 |
|`postForLocation`|`URI`| `String` url, `Object` request, `Map` uriVariables | url定义：　http://xxx.xxx?name={name}, `map`参数必须有一个key为`name`的键值对 |
|`postForLocation`|`URI`| `URI` url, `Object` request | 使用一个统一资源标识符 |

---

### PUT请求

| 方法名  | 返回　| 参数 | 说明 |
| --- | --- | --- | --- |
|`put`| `void` |`String` url, `Class` responseType, `Object`... urlVariables| 第三个参数是一个数组，顺序对应`url`占位符顺序 |
|`put`| `void` |`String` url, `Object` request, `Class` responseType, `Map` uriVariables| url定义：　http://xxx.xxx?name={name}, `map`参数必须有一个key为`name`的键值对 |
|`put`| `void` |`URI` url, `Object` request, `Class` responseType| 使用一个统一资源标识符 |

---

### DELETE请求

| 方法名  | 返回　| 参数 | 说明 |
| --- | --- | --- | --- |
|`put`| `void` |`String` url, `Object`... urlVariables| 第三个参数是一个数组，顺序对应`url`占位符顺序 |
|`put`| `void` |`String` url, `Map` uriVariables| url定义：　http://xxx.xxx?name={name}, `map`参数必须有一个key为`name`的键值对 |
|`put`| `void` |`URI` url| 使用一个统一资源标识符 |
