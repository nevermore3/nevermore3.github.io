####bitset使用详解（常见的应用是需要对海量数据进行一些统计工作的时候，比如日志分析，用户数统计等）

----------
#####bitset的成员函数：any()、none()、count()、size()、test()、set()、reset()、flip()、to\_ulong()、operator[]、to_string()

bitset 和数组非常相似， 但是和常规的数组相比， bitset在空间分配上做了优化，即每个元素仅仅占用 one bit（which is eight times less than the smallest elemental type in C++:char）,每个元素都可以独立的随机读取。bitset的参数必须是整形的数值，使用位的方式和数组区别不大

构造函数：  

	bitset<n> b; //b有n位，每位都为0，参数n可以为一个表达式
	如bitset< 5> a; 则a为“00000”；
	bitset<n>b(unsigned long u); //b有n位，并且用u进行赋值，如果u超过n位则顶端被截除。
	例如：bitset<5>b(5);  则b为“00101”

	bitset<10>b(string s); //b是string对象s中含有位串的副本。
	string bitval("10011");
	bitset<5> t(bitval)
	当用string对象初始化bitset对象时，从string对象读入位集的顺序是从右向左。即11001 00000

b.any() 判断b中是否存在值为1的二进制位，若有一个或多个二进制位为1，则返回true 否则返回false  
b.none() 若所有位都为0 则返回true ，否则返回false  
b.count() b中置为1的二进制的个数  
b.size()  b中二进制的个数  
b[pos] 访问b中在pos处的二进制位  
b.test(pos)  判断b中在pos处的二进制位是否为1  
b.set() 把b中所有二进制位都置为1  
b.set(pos) 把b中在pos处的二进制位置为1  
b.reset()  把b中所有的二进制位都置为0  
b.reset(pos)  把b中在pos处的二进制位置置为0  
b.flip()  把b中所有的二进制位逐位取反  
b.flip(pos) b中在pos处的二进制位取反  
b.to\_ulong() 用b中同样的二进制位返回一个unsigned long值

####SET 详解（用红黑树实现）

----------

#####Set成员函数有：begin()、end()、clear()、empty()、max\_size()、size()
set和map一样是关联式容器，用来存储同一数据类型的数据，在set中每个元素的值都是**唯一**的，并且系统能够根据元素的值自动进行排序.  

	set<int>s;
	s.insert(6);
	s.insert(3);
	s.insert(4);
	s.insert(9);
	s.insert(3);
	s.size() // 4
	s.begin();  //3   //
	set<int>::iterator it;
	for(it = s.begin(); it!=s.end(); it++) //中序遍历
		cout<<*it<<endl;
	//输出 3 4 6 9 从小到大自动排序好了
