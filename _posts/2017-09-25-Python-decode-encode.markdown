#### Python中 encode&decode&unicode之间的区别
---
- decode 解码  
- encode 转码  
- **字符串在Python内部的表示是unicode编码,因此，在做编码转换时，通常需要以unicode作为中间编码，即先将其他编码的字符串解码（decode）成unicode，再从unicode编码（encode）成另一种编码**

> decode的作用是将其他编码的字符串转换成unicode编码，如str1.decode('gb2312')，表示将gb2312编码的字符串str1转换成unicode编码。
encode的作用是将unicode编码转换成其他编码的字符串，如str2.encode('gb2312')，表示将unicode编码的字符串str2转换成gb2312编码。
总得意思:想要将其他的编码转换成utf-8必须先将其解码成unicode然后重新编码成utf-8,它是以unicode为转换媒介的
如：s='中文'  
  
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
如上面代码，str&str1&str2均为字符串类型(str) 给字符串操作带来的比较大的复杂性  

---
#### 文件读取问题
假如我们读取一个文件，文件保存时候，使用的编码格式，决定了我们从文件读取的内容的编码格式，For example ，我们从记事本新建立一个文本文件test.txt，编辑内容，保存的时候注意，编码格式是可以选择的，例如我们可以选择gb2312，则使用python读取文件内容，方法如下：  

	f = open('test.txt','r')
	s = f.read() #当读取文件内容的时候，如果是不识别的encoding格式(识别encoding类型和使用的系统有关系)，则会读取内容失败
	‘’‘ 假如文件保存时用gb2312编码保存’‘’
	u = s.decode('gb2312') #用文件保存格式对内容进行编码，以获得unicode字符串 gb2312 --->unicode
	‘’‘ 之后我们就可以对内容进行各种编码的转换了’‘’
	str = u.encode('utf-8') # unicode --->utf-8
	str1 = u.encode('gbk') # unicode ---> gbk
	str2 = u.encode('utf-16') #unicode ---> utf-16
	
python 也提供了一个包codecs进行文件的读取，这个包中的open()函数可以指定编码的类型。  

	import codecs
	f = codecs.open('test.txt', 'r+', encode = 'utf-8') #前提是必须知道文件的编码格式(文件原始是utf-8编码格式，然后python读取时候自动转换为unicode，这时在显示的指定编码为utf-8)
	content = f.read() #如果open时候使用的encoding和文件本身的encoding不一致的话，这里会发生错误。
	f.write('information that you want to write')
	f.close()
---
Example:  

	#coding=UTF-8
	if __name__ == '__main__':
	    s = '中国'
	    su = u'中国''
	    #s为unicode先转为utf-8
	    #因为s为所在的.py(# -*- coding=UTF-8 -*-)编码为utf-8
	    s_unicode =  s.decode('UTF-8')
	    assert(s_unicode == su)
	    #s转为gb2312:先转为unicode再转为gb2312
	    s.decode('utf-8').encode('gb2312')

