---
title: wequick下载地址
date: 2020-05-15 11:13:46
tags: wequick
---
20200119 最新版-带微信唤起功能
-----------------------------------
[下载](https://wequick-1257130190.cos.ap-shanghai.myqcloud.com/spec/WeQuick_Api_Setup.exe "https://wequick-1257130190.cos.ap-shanghai.myqcloud.com/spec/WeQuick_Api_Setup.exe")

[文档](https://github.com/juguang2018/wequick "https://github.com/juguang2018/wequick")

收到的参数加了一个，是pid，就是微及时唤起的微信的进程id，如果未唤起微信，pid默认是0



20200117 网页插件唤醒版
[网页唤醒版](https://wequick-1257130190.cos.ap-shanghai.myqcloud.com/CLEANAPI/WeQuick_Api_Setup.exe "https://wequick-1257130190.cos.ap-shanghai.myqcloud.com/CLEANAPI/WeQuick_Api_Setup.exe")

其实就一行，用a标签就行。<a?href="WeQuickProtocol:">打开WeQuick</ a>
消息格式
```
{
	"data": {
		"action": "reportTextMessage",
		"cwxid": "wxid_llnweznfkdeu12",
		"data": {
			"msg": {
				"msgType": 1,
				"myMsg": "0",
				"roomWxid": "8805044791@chatroom",
				"wxidFrom": "wxid_0qqelh74m78r22",
				"wxidTo": "8805044791@chatroom",
				"nick": "美好",
				"message": "专柜同款咱们会场都有哈，大家慢慢挑，只要遇到我们的价格，你到专柜确实不想下手了啦"
			}
		}
	}
}
```