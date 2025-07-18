Feed: 小众软件
Title: 学到了：localhost 居然支持二级域名
Author: 青小蛙
Date: Thu, 03 Jul 2025 17:18:14 +0800
Link: https://www.appinn.com/sub-localhost/
 
[image 1: 学到了：localhost 居然支持二级域名 4 (link #1)] 
 
在任意浏览器打开 http://localhost/[2] 实际上访问的就是本机地址，即 
127.0.0.1（如果你的设备上没有开启服务，那么还是打不开）。
 
内网地址不够怎么办
 
但如果家里有很多服务，用一个地址就很不够了。于是，localhost 还能这样用：
 
  * http://appinn.localhost
  * http://123.localhost
  * http://456.localhost
 
[image 2: 学到了：localhost 居然支持二级域名 5 (link #3)] 
 
不需要修改 hosts 文件，直接用就行。但是不支持 ping
[image 3: 学到了：localhost 居然支持二级域名 6 (link #4)] 
 
青小蛙测试 macOS 与 Windows，都没有问题，随开随用。

 ------------------------------------------------------------------------------ 

青小蛙也研究了下 RFC 6771[5] 文档，里面有一条，虽然看不懂，但可以这样用。
 
6.3 . “localhost” 域名预留注意事项
 
域“localhost.”以及任何属于“.localhost.”的名称在以下方面具有特殊性：
 
 1. 用户可以像使用其他域名一样自由使用本地主机名。用户可以假设对本地主机名的 
 IPv4 和 IPv6 地址查询始终会解析到相应的 IP 环回地址。
 2. 应用软件可以将本地主机名识别为特殊名称，或者可以像对待其他域名一样将其传递给
 名称解析 API。
 3. 名称解析 API 和库应该将本地主机名识别为特殊名称，并且应该始终为地址查询返回 
 IP 环回地址，为所有其他查询类型返回否定响应。
 4. 名称解析 API 不应将本地主机名称的查询发送到其配置的缓存 DNS 服务器。
 5. 缓存 DNS 服务器应该将本地主机名识别为特殊名称，并且不应该尝试查找它们的 NS 
 记录，或者以其他方式查询权威 DNS 服务器以尝试解析本地主机名。
 
相反，对于所有此类地址查询，缓存 DNS 服务器应该立即生成一个提供 IP 
环回地址的肯定响应，而对于所有其他查询类型，则立即生成一个否定响应。
 
这是为了避免对根名称服务器和其他名称服务器造成不必要的负载。

 ------------------------------------------------------------------------------ 

 
Links: 
[1]: https://do-cdn.appinn.com/static3/images/2025/07/Copy-of-appinn-homework-2025-07-03T153126.940.jpg (image)
[2]: http://localhost/ (link)
[3]: https://do-cdn.appinn.com/static3/images/2025/07/Screen-20250701172337@2x.avif (image)
[4]: https://do-cdn.appinn.com/static3/images/2025/07/Screen-20250701173007@2x.avif (image)
[5]: https://www.rfc-editor.org/rfc/rfc6761.html#page-8 (link)
[6]: https://mp.weixin.qq.com/s/_36t6AkJDK5pjFFdGRi48w (link)
[7]: https://www.appinn.com/sub-localhost/ (link)

