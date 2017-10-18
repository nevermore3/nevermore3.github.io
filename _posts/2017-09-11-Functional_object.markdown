#仿函数  =  函数对象

----------

####仿函数，就是使一个类或者结构体的使用看上去像一个函数，其实现就是类（结构体）中实现一个operator()，然后这个类（结构体）就有了类似函数的行为，就是一个仿函数类（结构体）了。

在c语言中使用函数指针和回调函数来实现仿函数，例如一个用来排序的函数可以这样使用仿函数


	int sort_function(const void *a, const void *b)
	{
		return *(int*)a - *(int*)b;
	}
	int list[5]={54,32,12,45,75};
	qsort((void*)list, 5,sizeof(list[0]), sort_function);
在c++中，可以通过一个类中重载括号运算符的方法使用一个函数对象而不是一个普通函数。
	
	template<typename T>
	class display
	{
		public:
			void operator()(const T &x)
			{
				cout<<x<<"\t";
			}
	}
	
	int ia[] = {1,23,43,234,6};
	for_each(ia,ia+5,display<int>())


###函数对象

----------
函数对象：如果一个对象具有的函数的某些功能，我们称之为函数对象，如何使得对象具有函数功能呢， 只需要为这个对象重载 操作符() 就可以了。 **函数对象可以携带附加数据，而函数指针则不行**。

	class A
	{
	public:
		int operator()(int x)
		{
			return x;
		}
	}
	A a;
	a(5);
这时候a就成为了一个函数对象，当执行a(5)的时候，实际上就是重载了 操作符().  
函数对象既然是一个“类对象”，则我们可以再函数形参列表中调用它，它可以完全取代函数指针！，当我们想在形参列表中调用某个函数时， 可以先声明一个具有这种函数功能的函数对象，然后在形参中使用这个对象，它所做的功能和函数指针所做的功能是一样的，而且更加安全。  

	class Func
	{
	public:
		int operator()(int a, int b)
		{
			cout<<a<<'+'<<b<<'='<<a+b<<endl;
			return a;
		}
	};
	int addFunc(int a, int b, Func &func)
	{
		func(a,b)
		return a;
	}
	Func func;
	addFunc(1,3,func);
上述例子中首先定义了一个函数对象类，并重载了()操作符，目的是使前两个参数相加并输出，然后在addFunc中的形参列表中使用这个类对象，从而实现两数相加的功能。

####函数对象和函数指针的区别
我们需要使用for\_each将一个vector中的每一个值加上某个值然后输出，若使用普通函数，则则其声明应该为void add\_num(int value, int num),其中value为容器中的元素，num为要加上的数。但是由于for\_each函数的第3个参数就要求传入接受一个参数的函数或函数对象,所以将add\_num函数传入for\_each是错误的，然而函数对象可以携带附加数据解决这个问题:

	class Add
	{
	public:
		Add(int num):num_(num){}
		void operator()(int value)
		{
			cout<<value+num_ <<endl;
		}
	private:
		int num_;
	}
	vector<int> v;
	v.push_back(2); v.push_back(23), v.push_back(43);
	Add add(2);
	//for_each的第3个对象为函数对象
	for_each(v.begin(), v.end(), add)
例子2，两个对象的比较

	class A
	{
		int n;
	public:
		A(int _n):n(_n){}
		bool operator()(const A&lhs, const A&rhs)
		{
			return lhs.n<rhs.n
		}
	}
