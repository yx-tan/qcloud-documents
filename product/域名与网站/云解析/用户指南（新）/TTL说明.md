TTL（Time to live），是指各地 DNS 服务器缓存解析记录的时长。

假设 TTL 设定为10分钟，当各地的 DNS 服务器接收到域名的解析请求时，会向权威服务器发出请求获取到解析记录，并在本地服务器保存10分钟，在10分钟内，解析请求将从本地缓存中读取，缓存失效后才会重新获取记录值。建议正常情况下设定10分钟即可，使用不同套餐版本的解析能设定的 TTL 最低值不同。

|解析套餐版本 | 最低 TTL |
|---|---|
| 免费版 | 600s |
| 专业版| 60 秒 |
| 企业版| 1s |
| 尊享版| 1s |

