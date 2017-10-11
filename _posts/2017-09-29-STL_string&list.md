####STL string&List 详解

----------
#####String 成员函数有：swap()、push\_back()、append()、insert()、erase()、clear()、repalce()、+、size()、length()、max\_size()、empty()、capacity()、reverse()、begin()、end()、substr()、find()

string 可以用来处理字符串文本信息

	ifstream in("name.txt")
	string strtmp;
	vector<string>vect;
	while(getline(in, strtmp, '\n') )
	vect.push_back(strtmp.substr(0,strtmp.find(' ')) );
**查找**
  
	str.find("ab");  //返回字符串ab在str的位置  
	str.find("ab", 2); //在str[2]~str[n-1]范围内查找并返回字符串在str中的位置    

**子串**  

	str.substr(3)  //返回[3]及其以后的子串  
	str.substr(2,4); //返回str[2]~str[2+(4-1)] 的子串，即从[2]开始4个字符组成的字串  

**替换**  

	str.replace(2,4,"sz") //返回把[2]~[2+(4-1)]的内容替换为“sz”后的新字符串。  
	str.replace(2,4,"abc",3) //返回把[2]~[2+(4-1)]的内容替换为“abcd”的前3个字符后的新字符串  

**插入**  

	str.insert(2,"sz")// 从[2]位置开始插入字符串“sz”，并且返回新的字符串  
	str.insert(2,"abc",3);//从[2]位置开始添加字符串 "abcd" 的前 3 个字符，并返回形成的新字符串

**追加**  
除了用重载的 + 操作符，还可以使用函数来完成。
	
	str.push_back('a') //在str的末尾添加字符'a'
	str.append("abc") //在str的末尾添加字符串“abc”

**删除**  

	str.erase(3) //delete [3]以及以后的字符串，并且返回新的字符串  
	str.erase(3,5) // delete从[3]开始的5个字符，并且返回新的字符串

**交换**  

	str1.swap(str2)  //把str1和str2交换

**其他**  
	
	str.size() str.length() //返回字符串的长度
	str.empty() //检查字符串是否为空，若空则返回1 否则返回0
	str[n] //存取str第n+1个字符

**Example**  
查找给定字符串并把相应子串替换为另一给定字符串,string 并没有提供这样的函数，所以我们自己来实现。由于给定字符串可能出现多次，所以需要用到 find() 成员函数的第二个参数，每次查找之后，从找到位置往后继续搜索。直接看代码（这个函数返回替换的次数，如果返回值是 0 说明没有替换）

	int str_replace(string &str, const string &src, const string &dest)
	{
	    int counter = 0;
	    string::size_type pos = 0;
	    while ((pos = str.find(src, pos)) != string::npos) {
	        str.replace(pos, src.size(), dest);
	        ++counter;
	        pos += dest.size();
	    }
	    return counter;
	}
	




####STL List详解

----------
#####List成员函数有：begin()、end()、assign()、front()、back()、empty()、size()、max_size()、clear()、insert()、erase()、push\_back()、pop\_back()、push\_front()、pop\_front()、reverse()、unique()、remove()、remove\_if()、sort()

List实际上是一个双向链表，可以高效的进行插入和删除元素。  
c.begin()  返回指向链表第一个元素的迭代器  
c.end()    返回指向链表最后一个元素之后的迭代器  

	list<int>a{1,2,3,4,5};
	list<int>::iterator iter;
	for(iter=a.begin(); iter!=a.end(); iter++)
		cout<<*iter<<endl;
c.assign(n, elem) 将n个elem元素拷贝赋值给链表c。  
c.assign(beg,end) 将[beg, end)区间的元素拷贝赋值给链表c。  

	int a[6]={23,34,12,545,64,32};
	list<int>a1;
	a1.assign(12,323);
	a1.assign(a,a+6);
c.front() 返回链表的第一个元素   
c.back() 返回链表的最后一个元素  

	list<int>a{12,23,5,7,0};
	int first_elem = a.front();
	int last_elem = a.end();
c.empty() 判断链表是否为空。  
c.size() 返回链表中实际元素的个数  
c.max_size() 返回链表可能容纳最大元素的个数。  
c.clear()  清除链表中的所有元素。  
c.insert(pos , num)  在pos位置插入元素 num。  
c.insert(pos, n, num) 在pos位置插入n个元素 num。  
c.insert(pos,beg,end) 在pos位置插入区间[beg,end)的元素。  

	int arr[5]={23,12,45,67,2};
	list<int> t;
	t.assgin(3,9);
	t.insert(t.begin(), 0);
	t.insert(t.begin(),2,34);
	t.insert(t.begin(), arr, arr+5);
c.erase(pos)  删除pos位置的元素  
c.push\_back(elem) 在末尾增加一个元素  
c.pop\_back()  删除末尾的元素  
c.push\_front(elem) 在开始位置增加一个元素  
c.pop\_front()  删除第一个元素
c.resize(n)  重新定义链表的长度，超出原始长度部分用0代替，小于原始部分删除  
c.resize(n, elem) 重新定义链表的长度，超出部分用elem元素代替  
c.remove(elem)  删除链表中元素值等于elem的元素  
c.reverse()  反转链表  
c.sort()  链表排序，默认是升序  
c.sort(compare)  自定义回调函数实现自定义排序。
