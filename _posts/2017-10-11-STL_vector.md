###C++ STL 容器详解
####Vector 使用详解
----------
#####主要的成员函数有 size()、capacity()、reverse()、resize()、push\_back()、pop\_back()、assign()、begin()、end()、clear()、empty()、max\_size()、erase()
vector的存储特性：vector是一个容器，它能够像容易一样存放各种类型的对象，是一个动态数组，能够增加和压缩数据  (**底层是用数组实现的**)

c.push_back(elem)在尾部插入一个elem数据  
	
	vector<int>v ;
    v.push_back(1);
c.pop_back()删除末尾的数据。

	vector<int>v;
	v.pop_back();
c.assign(beg,end) 将[beg,end)区间的数据赋值给c

	vector<int> v1,v2;
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v2.assign(v1.begin(),v1.end());
c.assign(n,elem)将n个elem元素拷贝赋值给c

	vector<int> c;
	c.assign(5,20); //向c中放5个20

c.capacity() 返回容器的大小，翻倍增长


	vector<int>v;
	v.push_back(1);
	cout<< v.capacity()<<endl; //1
	v.push_back(2);
	cout<< v.capacity()<<endl; //2
	v.push_back(3);
	cout<< v.capacity()<<endl; //4
c.begin() 返回指向第一个数据的迭代器，c.end()返回指向最后一个数据的迭代器

	vector<int>v;
	v.assgin(13,4);
	vector<int>::iterator iter;
	for(iter = v.begin(); iter!=v.end(); iter++)
	{
		cout<<*iter<<endl;
	}
c.clear() 移除容器中所有的数据

	vector<int>::iterator it;
	for(it=v.begin(); it!=v.end(); it++)
		cout<<*it<<endl;
	c.clear();
	for(it=v.begin(); it!=v.end(); it++)
		cout<<*it<<endl;
c.size() 返回当前容器元素的个数

	vector<int>c;
	c.assgin(13,9);
	cout<<c.size()<<","<<c.capacity()<<endl; /13,13
	c.clear()
	cout<<c.size()<<","<<c.capacity()<<endl; /0,13
c.empty() 判断容器是否为空
	
	vector<int>c;
	c.assgin(13,13);
	if(!c.empty())
		cout<<"c is not empty!"<<endl;
c.resize() 改变容器的大小，并且创建对象，因此，调用这个函数之后，就可以引用容器内的对象  
c.reserve是容器预留空间，并不真正创建对象，在创建对象之前，不能引用容器内的元素。

	vector<int> v;
	v.assign(50,34); // 当前size()为50， capacity()为100
	v.resize(10) //size==10, 10到49下标的元素被delete，capacity==100，没有进行内存重新分配
	v.resize(60) //size==60,50到59下标用默认构造函数填充，capacity==100,没有进行内存重新分配
	v.resize(200，321) //size==200, capacity==200, 50到199下标元素用321填充， 自动扩容，重新分配内存。
	v.reserve(10) //size==50 没有元素被delete，capacity==100, 即reserve 没有起作用
	v.reserve(60) //size==50，没有元素被delete，capacity==100, reserve没有起作用
	v.reserve(300) //size==50 ，没有元素被delete，capacity==300, 自动扩容，内存重新分配。
c.max_size() 容器最大可以分配多大的空间。
	
	vector<int>c;
	cout<<c.max_size()<<endl; // 最大可分配 1073741823 即超过10亿个元素，程序就会崩溃。（不同计算机，数值不同）
c.erase(pos)删除pos位置的数据，传回下一个数据的位置

	vector<int>c;
	c.assgin(12,432);
	c.earse(c.begin());
c.erase(beg,end)删除[beg,end)区间的数据，传回下一个数据的位置。

	vector<int>v;
	v.assgin(12,32);
	v.earse(v.begin(), v.end());


###FAQ
#####STL中迭代器详解（STL为每个容器都定义了对应的迭代器）

----------
 标准迭代器的语法被设计为类似于普通C指针算术的语法，其中使用 * 和 -> 运算符来引用迭代器所指向的元素，并且使用诸如++的指针算术运算符将迭代器推送到下一个元素. 迭代器通常成对使用，其中一个用于实际的迭代，第二个用于标记集合的结束。迭代器由相应的容器类使用诸如begin（）和end（）之类的标准方法创建。由begin（）返回的迭代器指向第一个元素，而由end（）返回的迭代器是不引用任何元素的特殊值。当迭代器超出最后一个元素时，它的定义等于特殊的结束迭代器值。(所以我们说end（）指向的是最后一个元素的后一位）。综上所述 iterator 像是一个比较聪明的 pointer ， 它可以指到容器内任何一个位置，然后操作那个位置的资料。  
如下图所示： 
 
![](http://i.imgur.com/ZXJimgv.png) 
 
这个图代表一个栈，base指针一直指向我们的最底层元素A，top指针一直指向最后一个元素C的上端。这个时候我们遍历这个栈，那么我们stack.begin()就相当于我们的base指针，stack.end（）就相当于我们的top指针。 
