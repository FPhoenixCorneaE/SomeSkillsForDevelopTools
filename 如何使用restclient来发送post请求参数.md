## 如何使用restclient来发送post请求参数？	

​	

	运行 restclient ，点选Method选项卡的“POST”方法。然后选择增加HTTP头字段选项卡，
	下拉列表中选择”自定义HTTP头字段“的选项，配置上 名称：Content-Type  值：application/json;charset=UTF-8，
	再在正文里面写入body字符串，也就是你的请求条件，如：query=xpsF
	这样就可以传递post的参数了。



	使用restclient的请求为 ：POST
	
	HTTP头字段为： Content-Type:application/json;charset=UTF-8
	
	body正文内容为Json：{"vname":"13104871646","pwd":"dc483e80a7a0bd9ef71d8cf973673924","token":"abff7e53d00791c340a17fa80c76075f","tt":"1522737531.9297","vcode":"hqxe"}
	
	这样后台就能收到对象了。