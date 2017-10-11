##hash\_map/unordered\_map详解(查找时间复杂度是常数级别的)
----
hash\_map基于hash table(哈希表)实现，其优点就是把数据的存储和查找消耗的时间大大降低，而代价就是消耗比较多的内存。  

	unordered_map<int, string> map;
	map.insert(make_pair(1,"scala"))
	map.insert(make_pair(2,"haskell"))
	map.insert(make_pair(3,"C++"))
	map.insert(make_pair(4,"java"))
	map.insert(make_pair(5,"erlang"))

	unordered_map<int, string>::iterator it;
	if( ( it = map.find(6))! = map.end())
		cout<<it->second<<endl;

hash表的基本原理： 使用一个下标范围比较大的数组来存储元素， 设计一个函数（哈希函数，也叫散列函数），value= hash(要存储的元素），value的值对应数组的下标，使用这个数组单元来存储这个元素，有肯可能两个不同的元素计算出了相同的值， 所以“直接定址”和“解决冲突”是哈希表的两大特点。  
hash\_map，首先分配一大片内存，形成很多桶，利用hash函数，对key映射到不同的桶中进行保存，插入的过程是：  
	1.  得到key  
	2.  通过hash函数得到hash值  
	3.  得到桶号（一般都是hash值对桶数求摸）  
	4.  存放key和value在桶内。  
其取值过程是：  
	1.  得到key  
	2.  通过hash函数得到hash值  
	3.  得到桶号（一般都是hash值对桶数求摸）  
	4.  比较桶的内部元素是否与key值相同，若不相等，则没有找到  
	5.  取出相等的记录value    
	

----------
hash\_map 探析  


	hash_map<int, string, hash<int> , equal_to<int> > mymap;
	第三个和第四个参数是函数对象，SGI STL 提供了以下hash函数
	struct hash<char *>
	struct hash<const char*>
	struct hash<char>
	struct hash<unsigned char>
	struct hash<signed char>
	struct hash<short>
	struct hash<unsigned short>
	struct hash <int>
	struct hash<unsigned int>
	struct hash<long>
	struct hash<unsigned long>
即，如果key的使用是以上类型的一种，都可以使用默认的hash函数，当然也可以定义自己的hash函数。例如对于string：  

	struct str_hash
	{
		size_t operator()(const string &str) const
		{
			unsigned long _h=0;
			for(size_t i=0; i<str.size(); i++)
				_h = 5*_h+str[i];
			return size_t(_h);
		}
	};
	
	重载 比较函数
	
	struct compare_str
	{
		bool operator()(const char *p1, const char*p2)
		{
			return strcmp(p1,p2) == 0;
		}
	}
则我们可以使用自己定义的hash\_map（）了  
  
	typedef hash_map<const char *, string, hash<const char*>, compare_str> StrMap;
	StrMap namemap;

**如何在hash\_map中加入自己定义的类型：1、定义hash函数。2、定义等于比较函数。**
