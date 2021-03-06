<!--
 * @Description  :
 * @Author       : YH000052
 * @LastEditors  : YH000052
 * @Date         : 2020-06-21 11:13:30
 * @LastEditTime : 2020-06-23 14:58:29
 * @FilePath     : \notes\notes\Browser-caching-mechanism.md
-->

# Browser caching mechanism

因为之前项目中有出现前端缓存问题, 所以针对这个问题想整理一下, 记录一些项目中遇到的问题

## 缓存过程分析

[缓存过程分析]('../assets/images/cache/cache-1.jpg')

```bush
浏览器 ---------------浏览器缓存 ----------------- 服务器
1. 请求页面资源 => 询问浏览器中是否存在缓存 => /
2. 发起http请求 => 服务器(询问当前缓存是否新鲜)
3. 如果当前资源是新鲜的, 服务器返回304与当前缓存的标识
4. 浏览器继续使用缓存中的资源
5.
```

## 缓存类型

### 强制缓存

> 强制缓存的规则: 当浏览器向服务器发起请求时，服务器会将缓存规则放入 HTTP 响应报文的 HTTP 头中和请求结果一起返回给浏览器，控制强制缓存的字段分别是 Expires 和 Cache-Control，其中 Cache-Control 优先级比 Expires 高

- expires
  如果当前客户端的时间小于 expires, 则会继续使用缓存, 否则重新发起请求
- cache-control
  - public 全部使用缓存
  - private 仅客户端使用缓存
  - no-cache 全部不使用缓存
  - no-store max-age=timestamp 根据 stamp 来决定缓存多久后过期

强制缓存就是向浏览器缓存查找该请求结果，并根据该结果的缓存规则来决定是否使用该缓存结果的过程，强制缓存的情况主要有三种(暂不分析协商缓存过程)，如下：

1. 不存在缓存结果和缓存标识, 强制缓存失效, 重新发起 http(与第一次发起请求一致)

2. 存在缓存结果和缓存标识, 结果失效， 强制缓存失效, 重新发起请求

3. 存在缓存结果和缓存标识, 结果有效, 继续使用缓存

### 协商缓存
