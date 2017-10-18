###C++ STL 容器详解
####deque 使用详解

----------
#####主要成员函数有 assgin()、at()、back()、begin()、clear()empty()、end()、erase()、front()、insert()、 max\_size()、pop\_back()、pop\_front()、push\_back()、push\_front()、resize()、size()、swap()
deque称之为双向队列容器，和vector相似，支持随机访问和快速插入删除，vector与 deque内部数据管理的根本不同。 deque的内存空间分布是小片的连续，小片间用链表相连，实际上内部有一个map指针，deque空间的重新分配比vector要快，重新分配空间后，原有的元素是不需要拷贝的。，deque的内存所示如下：  
![](http://images.cnitblog.com/blog2015/277239/201503/211659037977315.gif)  
采用一小块连续的内存索引缓存结点，每一个缓存结点也是一段连续的空间，可以存储多个数据。当索引内存空间满载，需要申请一块更大的内存做索引。 
  

c.begin() 返回指向第一个元素的迭代器  
c.end() 返回指向最后一个元素下一个位置的迭代器

	deque<int>d {1,2,3,4,5};
	deque<int>::iterator iter;
	for(iter = d.begin(); iter!=d.end(); iter++)
		cout<<*iter<<endl;
c.assign(n, elem)将n个elem元素拷贝复制到容器c中  
c.assgin(beg,end) 将[beg,end)区间的数据拷贝复制到容器c中  
c.empty() 判断c容器是否为空  
c.front() 返回容器的第一个元素  
c.back() 返回容器的最后一个元素

	deque<int>d{1,2,3,4,5};
	if(!d.empty())
	{
		cout<<d.front()<<endl;
		cout<<d.back()<<endl;
	}
c.size() 返回容器c中实际拥有元素的个数  
c.max\_size() 返回容器可能存放元素的最大个数  
c.clear() 清除容器中的元素
c.insert(pos,num)在pos位置插入元素num  
c.insert(pos,n,num)在pos位置插入n个元素num  
c.insert(pos,beg,end)在pos位置插入区间为[beg,end)的元素

	int a[]={1,2,3,4,5};
	deque<int>d;
	d.insert(d.end(),22)
	d.insert(d.end(),3,23)
	d.insert(d.begin(), a, a+3)
  
c.push\_back(num)在末尾位置插入元素  
c.pop\_back()删除末尾位置的元素  
c.push\_front(num)在开头位置插入元素  
c.pop\_front()删除开头位置的元素

####Queue 使用详解
----------
#####主要成员函数有empty()、back()、front()、pop()、push()、size()、swap()
Queue是一种运算受到限制的线性表，在底层默认是基于deque（双端队列）来实现的。插入只能在表的一端进行（只进不出），而删除只能在表的另一端进行（只出不进），如下图所示：  
![](http://img.blog.csdn.net/20170326153249670?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcmVkUm50/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
q.push(elem) 入队，将x插入到队列的末端

	queue<int>q;
	q.push(2);
q.pop(elem) 出队，弹出队列的第一个元素，但是不会返回被弹出元素的值

	queue<int>q;
	q.push(3);
	q.push(32);
	q.push(321);
	p.pop();   //3 被弹出
q.empty() 判断对垒是否为空，当为空时返回True，否则返回False  
q.size() 返回队列中元素的个数  
q.front() 访问并返回队首元素，  
q.back()  访问并访问队尾的元素。

####Stack 使用详解
----------
#####主要成员函数有 pop()、top()、push()、size()、empty()、 emplace()  
stack默认是基于deque（双端队列）实现的，插入和删除都只能在一端进行，有”先进后出“的特性。
![](http://img.blog.csdn.net/20170326153225515?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcmVkUm50/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
stack虽然是基于deque实现的，但是我们也可以传入参数，使其在底层使用其他的容器来实现比如：  

	stack<int, list<int>>  //使用list实现
	stack<int, vector<int>> //使用vector实现
	stack<int, array<int>> //使用数组实现
s.push(elem) 向栈中添加元素 ，没有返回值

	stack<int> s;
	s.push(3);
s.pop() 从栈头delete第一个元素。没有返回值

	stack<char> s;
	s.push('a')
	s.push('b')
	s.push('c')
	s.pop()
s.top() 从栈顶返回第一个元素的值

	stack<char> s;
	s.push('a')
	s.push('b')
	s.push('c')
	char t=s.top()   /t=='c'
s.empty()  判断栈是否为空，若空返回True否则返回False  
s.size() 返回栈内元素的个数

####priority_queue详解
----------

#####主要成员函数有empty()、size()、top()、push()、pop()
priority\_queue 底层默认是用vector实现的，优先对列是一种抽象的数据类型，行为和队列类似，但是先出队的元素不是先进队列的元素，而是队列中优先级最高的元素。(一般大根堆和小根堆会使用优先队列)  
priority\_queue的模板原型是：   
priority\_queue<Type,Container, Functional>   
Type:     存放容器的元素类型  
Container:实现优先队列的底层容器，默认的是vector<T>  
Functional:用于实现优先级的比较函数，默认的是functional中的less<T>  
其实现可以如下所示：

	class priority_queue
	{
		private:
			vector<int>data;
		public:
			void push(int t)
			{
				data.push_back(t);
				push_heap(data.begin(), data.end());
			}
			void pop()
			{
				pop_heap(data.begin(), data.end());
				data.pop_back();
			}
			int top() {return data.front()}
			int size() {return data.size()}
			int empty() {return data.empty()}
	}

由于STL的priority_queue 默认用的是vector，默认的比较方式是 operator < 所以如果后面两个参数缺省的话，优先队列就是大根堆，队头元素最大。如果用到小根堆，则一般要把模板的三个参数都带进去。

#####自定义比较函数

	struct cmp
	{
		bool operator()(const int &a, const int &b)
		{
			return a>b //大根堆
			return a<b //小根堆
		}
	};
	
	priority_queue<int, vector<int> , cmp>A;

----------
将结构体以val由大到小排序，形成大根堆

    struct Node
	{
		int adj;
		int val;
	};
	
	struct cmp
	{
		bool operator()(const Node &a, const Node &b)
		{
			return a.val > b.val;
		}
	}

	priority_queue<Node, vector<Node>, cmp>Q;

----------
第一个值由大到小排序，第二个值由小到大排序  

	struct Node
	{
		int first;
		int second;
	};
	struct cmp
	{
		bool operator()(const Node &a, const Node &b)
		{
			if(a.first > b.first)
				return true;
			else if (a.first == b.first)
			{
				return a.second < b.second;
			}
			else
				return false;
		}
	};

	priority_queue<Node, vector<Node>, cmp> Q;

