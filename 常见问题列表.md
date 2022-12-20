## 1.OpenAI官网注册提示 Not avaliable

一般是vpn未生效，注意地区要选择韩国、美国等，如果切换几个地区都不行就试试清除浏览器缓存，或是用无痕页面打开。账号注册的参考该[博客]。(https://www.cnblogs.com/damugua/p/16969508.html)


## 2.项目启动报错SSL连接失败

```
During handling of the above exception, another exception occurred:
requests.exceptions.ConnectTimeout: HTTPSConnectionPool(host='webpush.wx.qq.com', port=443): Max retries exceeded with url:
you can't get access to internet or wechat domain, so exit.
```
可能有两个原因：
1. 网络问题，用浏览器打开[网页微信](https://login.weixin.qq.com/) 看看能否能访问，检查下电脑是否挂了vpn，如果是的要关掉后再登录。
2. Python版本过高 (3.10 或 3.11)，建议使用 3.7.1 ~ 3.9 版本的Python。


## 3.登录报错XML解析失败

```
expatbuilder.py", line 223, in parseString
    parser.Parse(string, True)
xml.parsers.expat.ExpatError: mismatched tag: line 64, column 4
```
检查是否安装了 itchat-uos，以及版本是否为 1.5.0.dev0

## 4.登录报错 KeyError:'wxsid'
```
login.py", line 183, in process_login_info
    core.loginInfo['wxsid'] = core.loginInfo['BaseRequest']['Sid'] = cookies["wxsid"]
KeyError: 'wxsid'
```
一般原因为使用了itchat且无法登录网页版微信，解决方法是先卸载itchat，然后安装itchat-uos 1.5.0.dev0版本。

## 5.登录报错 IndexError: list index out of range
```
login.py", line 197, in process_login_info
skey = re.findall('(.*?)', r.text, re.S)[0]
IndexError: list index out of range
```
一般原因是微信没有实名认证，前往支付板块进行实名认证后再登录。

## 6.登录超时二维码刷新Log in timeout
```
Log in time out, reloading QR code.
```
这种情况多发生于linux服务器上，原因是手机扫码后有异地登录验证，会等待5s，而此时itchat判断登录超时，又刷新了二维码，导致一直登录不上。
解决办法是修改 itchat的 login.py 源码，详细步骤参考 [Issue#8](https://github.com/zhayujie/chatgpt-on-wechat/issues/8)

## 7.登录成功但无法触发自动回复

一般原因是没有收到触发自动回复的文本内容，检查下config.json中的配置，个人聊天需发送以`single_chat_prefix`配置为开头的内容 (自身发送也可触发)；群组聊天需发送以 `group_chat_prefix`配置中 为开头的内容，或被 @ 也会直接触发。
