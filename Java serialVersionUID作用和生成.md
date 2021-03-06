序列化和反序列化
==============

Java是面向对象的语言，与其他语言进行交互（比如与前端js进行http通信），需要把对象转化成一种通用的格式。
比如json（前端显然不认识Java对象），从对象到json字符串的转换，就是序列化的过程，反过来，从json字符串
转换成Java对象，就是反序列化的过程。

---------------------------

serialVersionUID是什么？
=========================

反序列化的过程，需要从一个json字符串生成一个Java对象。典型的如下：

Gson gson = new Gson();
Request req = gson.fromJson("request string", Request.class)

这时候会有问题，需要验证输入的json字符串是否是从当前的Request这个类序列化过去的，serialVersionUID就是用来干这个的。
当序列化的时候的serialVersionUID与反序列化的时候的serialVersionUID不一致的时候，会跑出InvalidCalssException。 
如果没有显式地定义一个serialVersionUID，那么Java会默认根据类信息计算一个serivalVersionUID出来。

---------------------

如何生成
===========

Android Studio可以自动为serializable的类生成一个serialVersionUID。 
Settings - Editor - Inspections - Serializable class without ‘serialVersionUID’ 勾选
								- 'serialVersionUID' field not declared 'private static final long' 勾选。 
这样在没有serialVersionUID的类中，可以自动根据提示生成serialVersionUID了。

---------------------
