#### C++的智能指针详解
-------
1、 什么是指针  
2、 都有哪种类型的指针  （函数类型指针、对象类型指针）  
3、 C语言的指针  
4、 C语言指针的缺点  
5、 C++中的指针  
6、 为什么要引入智能指针  
7、 智能指针  

----------------------
##### 什么是指针
指针是对内存区域的抽象  
C 和 C++ 中的指针，是一种特殊的复合类型。指针变量中存放着目标对象的内存地址，而与指针相符合的类型，则说明了相应内存区域中的内容具有哪些属性，以及能做什么事情。也就是说，在内存空间某块区域中的内容，原本可以是不可解读的；但是，如果有一个描述这块内存区域的指针存在，我们就能找到它（地址的作用），并且合理地使用它（类型的作用）。因此，我们说：指针是对内存区域的抽象。


区分不同语言里变量的区别时，可以从三个角度来分析。  
1、变量名： 就是只能包含数字和字母之类的标示符  
2、数据类型： 就是该变量的数据类型(为了确定在取数据的时候取出多少字节)，比如`int double float int*`等等。   
3、值：实际存储在地址空间里面的值， 对于指针来说就是 存储在指向地址空间里面的值。  

**在C中，因为类型和变量名是绑定在一起的，所以数据类型也叫变量类型。而变量类型是可以通过强制类型转换来将变量存储的值转换为其他类型的，（即在C中，值和类型是分离的）然而在python中，则有所区别，Python的类型是与值绑定在一起的，而变量名只是一个名字，可以指向一个具体的值，而值的类型存储在其对象的结构体里面。这种名字和值的分离使得Python的动态性提高，编程更加容易，同事因为需要每次判定一个对象的类型而降低了性能。**  
在C中定义一个变量 `int a = 3` 而python中是`a = 3`
  

指针为什么要有类型  
类型系统有两个重要的作用，一个是用作编译时的类型检查，让编译器帮助我们避免犯错，另一个作用是，指明一个内存地址所保存的二进制数据，应该怎样被解释，比如在没有指针类型的情况下，虽然我们知道 a 中保存的地址，但是当我们想要解引用指针(*a),读出这块地址中的一个int类型数据时就有问题， 编译器怎么知道你从这个位置开始要读出一个字节，还是4个字节？即类型规定了指针移动的粒度。而在有int \*这个类型的指导下，编译器指导在解引用的时候要读出4个字节，甚至，我们也可以通过强制类型转换读出一个char类型的数据。

指针的主要功能有两个： [避免副本和共享数据](https://www.zhihu.com/question/31022750)  

---------
##### 都有哪些类型的指针  
1、 函数指针  
实际上C/C++语法上函数是没有办法直接调用的，能直接调用的就是函数指针，你平时调用函数会触发一个隐藏的函数->函数指针的转换，你对函数解引用后调用，结果还会重新获取函数指针再调用。 函数给函数指针赋值也是一样的，会自动转换成函数指针。也就是加不加& 其实都一样，一个隐式转换，一个显式转换。  
	
	void (*funP)(int);
	void myFun(int x)
	{
		printf("myFun : %d\n", x);
	}
	funP = myFun  or funP = &myFun
	funP(20);  // 或者 (*funP)(20);

1. 其实，myFun函数名和funP函数指针都是一样的，即都是函数指针。myFun函数名是一个**函数指针常量**， 而funP是**函数指针变量**。
2. 函数名调用如果都用(*myFun)(20)这样，那书写读起来不方便，所以C语言设计者才设计成可以允许myFun(10)这种形式调用，(和数学上的函数形式一样)
3. 为了统一调用方式，funP函数指针变量也可以funP(10)的形式调用
4. 函数指针变量也可以存入一个数组内， 数组的声明方法：int (*fArray[10])(int);

而为什么 funP = myFun 和 funP = &myFun、 funP(20)和(*funP)(20)表现出来的效果是一样的？  
这是因为在C语言层面，函数指针和其他指针之所以表现不同，其根本原因在于前者是指向函数而后者是指向数据，C语言中函数和数据是完全不同的东西。他们有完全不同的类型描述（C中函数没有类型，函数在系统层面是以函数签名来区别的），并且他们编译后分布在完全不同的内存段，执行的时候也是由完全不同的寄存器来控制的。  
具体来说， 如果我们有一个数据指针，对其解引：  
`int i = *j`  
在汇编层面的表示是这样的：  
`mov   ecx,  [eax] ;   //ecx: i, eax: j, [eax]: *j`  
但是如果我们有一个函数指针，对其解引：  

	typedef int (*myfunction)(int a , int b);
	myfunction f1 = func;   //good
	myfunction *f2 = &func;  // 无法编译
    myfunction f3 = &func;

在汇编层面的表示是这样的：  
`mov  ecx,  [eip-0x5d]  ;   //[eip-0x5d]: func, ecx: f1 or f3`  
情况就会稍复杂了，这里主要有三个问题： 
 
- 为什么 func 本身已经是一个地址了？
- 为什么既然 func 本身就是地址了，我们对 func 进行二次显式取址无法得到一个指针的指针？
- 为什么既然 func 本身就是地址了，我们对 func 进行二次显式取址仍然得到的是一个指针？

因为函数在编译后都会存放在代码段里，C语言在编译期需要确定所有的函数调用，像 eip-0x5d是在编译期就确定的恒定值，但是如果我们能对func二层取址，然后再使用二层解引去调用，函数调用编译期可知的前提就破坏了，因为eip-0x5d本省在编译期是不可知的。


---------

##### 二维数组和二级指针的区别  
定义一个二维数组 `int a[2][3] =[[1,2,3], [4,5,6]]`  
定义一个二级指针 `int **array_ptr`  
是否可以使用 `array_ptr = a` 赋值手段通过`array_ptr`来调用二维数组`a`里面的元素  
ANSWER： No 分析如下  
`a`的值是什么？ a的值是一个指向a这个数组第一个元素的指针，那么a这个数组的第一个元素是什么 ？ 第一个元素 还是一个数组，所以 **a 应该是一个指向一维数组的指针**，并且这个一维数组还是一个整形数组。  
而上面声明的 `int **array_ptr` 是一个指向整形的指针的指针，假设某个整形指针 `int *p` 那么 `int **array_ptr = &p`是可以成立的。也就是 `*array_ptr`引用的是整形指针， 而用`*array_ptr` 来引用一个数组指针，肯定就出错了。   
可以改正如下：  
`int (*array_ptr)[3] = a;`,即`array_ptr`是一个指向数组的指针，而`int b[3] = {1,2,3} int *t_ptr = b`中的`t_ptr`是一个指向整形元素的指针，**(虽然`array_ptr`和`t_ptr`里面存储的都是地址，但是所存储的类型不同，`array_ptr`指向的是一个 一维数组类型，而`t_ptr`指向的是一个 整形类型，这在计算机层面上就有本质的区别， 因为计算机根据所存数据的类型，来取对应类型大小的数据并且进行处理，在机器看来，它是一个一维int数组的地址，而不是一个int变量的地址了)**


----------
##### 什么是智能指针  
对于编译器来说，智能指针实际上是一个栈对象，并非指针类型，在栈对象生命周期即将结束时，智能指针通过析构函数释放有它管理的堆内存，所有智能指针都重载了"operator->"操作符，直接返回了对象的引用，用以操作对象，访问智能指针原来的方法则使用 “.”操作符。

#####  为什么要引入智能指针  
因为C++没有垃圾回收机制，所有的动态内存释放都是由程序员负责的，很容易造成内存泄露，为了管理动态内存，引入了之智能指针，它是一种行为类似指针的类，但是能够管理自己负责的内存区域。当对象离开作用域时能够释放内存，防止内存泄露。举个栗子如下：  

	void remodel(std::string &str)
	{
		std::string *ps = new std::string(str);
		...
		if(weird_thing())
			throw exception();
		str = *ps;
		delete ps;
		return ;
	}
当出现异常时候，delete不被执行，导致内存泄露。

--------
智能指针