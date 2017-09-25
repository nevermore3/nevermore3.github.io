#####Python中 encode&decode&unicode之间的区别
---
- decode 解码  
- encode 转码  
在python中，我们使用decode()和encode()进行编码和解码 ，其中python使用unicode类型作为编码的基础类型。可以表示如下：  

	    decode -------------------------encode  
	    str ----------->unicode---------->str 

```
u = u'中文' #显示指定unicode类型对象u
str = u.encode('gb2312') #以gb2312编码对unicode对像进行编码
str1 = u.encode('gbk') #以gbk编码对unicode对像进行编码
str2 = u.encode('utf-8') #以utf-8编码对unicode对像进行编码
u1 = str.decode('gb2312')#以gb2312编码对字符串str进行解码，以获取unicode
u2 = str.decode('utf-8')#如果以utf-8的编码对str进行解码得到的结果，将无法还原原来的unicode类型

```