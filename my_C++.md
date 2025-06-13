```
C++为什么编译时间那么长?
	因为模板的存在：模板的类型处理展开，还有模板元编程，直接把运行期的计算提前到编译期。
```



C++：语法：新的概念关键字等    编程思想：

面向对象：封装 继承 多态
STL库：模板库

强转：
静态强转：static_cast<int>(c1); -- 将c1强转成int 括号跟要转的变量或表达式
const
reinterpret
 

引用：取别名 两个变量指向同一片内存
int aa = 100;
//引用的定义，就表示给变量aa取了个别名，引用必须初始化
int& ref_aa = aa;
此时操作ref_aa那么aa的值也会改变

引用作为函数的形参，改变他就是改变实参，但是实参传的是变量的本身，不是地址
引用作为函数的返回值 要确保函数结束后生命名周期仍然存在

引用就是操作变量的同一片地址 可以理解为与指针类似



内联函数在编译的时候用函数体的内容替换函数的调用，提升程序效率
具有数据类型的概念，运算符的优先级

函数重载：
	就是多个函数都可以同名，用形参个数类型顺序等来区分，不可用过函数的返回值来区分

**在类里面的成员函数重载的话，有无virtual都不影响，意思就是你重载一个函数之后，可以在该函数之前用virtual声明为虚函数，也实现了函数的重载**

泛型编程（模板）：
	template<typename T>

函数缺省参数：
	void prnit(string s="---------")
	{
		cout << s <<endl;
	}


此时调用参数不传参将会输出--------


	void printf()


函数缺省第一个没缺省后面就不能缺省 只能从前往后缺省，中间或后面不能缺

名字空间：
	作用域访问符 ::   std::cout

​			这里：std:名字空间

​						:: :作用域访问符

​						cout:变量

局部作用域 全局作用域 

两个文件之间的函数可以直接声明了使用
变量的话要extern

空指针：
	nullptr ：规范写法 C语言只是一个宏定义



malloc new

```c++
malloc库函数
new运算符，可以重载
int *p = new int;
int *p = new int[100];
delete [] p;
p = nullptr;
```

名字空间

```c++ 
使用名字空间最好使用全路径
	::a  表示使用最上层的global也就是全局的a
名字空间 + :: + 变量或者函数

使用整个名字空间：
	using namespace std;
使用名字空间的某一个变量：
	using std::cout;
全路径：
	std::cout << "a is" << a << std::endl;
	
可嵌套：
    namespace MySpace
	{
    	int a = 10;
    	void fun()
        {
            std::cout << "hello " << std::endl;
        }
    	namespace MySpace{
            int a = 10;
        void fun()
        {
            std::cout << "hello " << std::endl;
        }
        }
    
	}
可重名,就可以理解成一个函数内部吧
    std::cout << "MySpace a " << MySpace::a << std::endl;
	std::cout << "MySpace::MySpace::a " << MySpace::MySpace::a << std::endl;

	std::cout << "MySpqce fun " << MySpace::fun() << std::endl;
	std::cout << "MySpace::MySpace::fun" << MySpace::MySpace::fun() << std::endl;
```

设置输出格式

```c++
十六进制输出
cout.setf(ios::hex,ios::basefield);

一下两个只能一次起效：
输出宽度
cout.width(n);
当输出达不到输出宽度时，填充字符
cout.fill('*');

对齐方式：
cout.setf(ios::left,ios::adjustfield);
left right 
    internal:数据符号位左对齐 数据为右对齐 中间为填充

清除格式：(一个一个的取消)
        cout.unsetf(ios::showbase);//取消前缀
		cout.unsetf(ios::basefield);//进制
        cout.unsef(ios::showpos);//


设置精度(输出位数):
	cout.precision(10);//输出十位
	cout.setf(ios::fixed,ios::floatfield);//
	科学计数法输出：
    cout.setf(ios::scientific,ios::floatfield);//科学计数法输出


```

预定义操作符

```c++
头文件
#include <iomanip>

int aa = 10;
cout << hex << aa << endl;
cout << std::hex << aa << endl;



```

面向过程到面向对象

```c++
封装：将行为封装到数据里面


class：类
	成员访问限定符：private protected public
	private:表示私有的成员变量和函数 不能通过类的对象直接访问
	protected:表示保护的成员变量和函数 不能通过类的对象直接访问
	public:表示公有的成员变量和函数 能通过类的对象直接访问

        
   eg:怎么访问私有和保护的成员变量以及成员函数
#include <iostream>
using namespace std;

class studen{
//成员访问限定符 private public protected

    private:
        int a;
        void fun();
    protected:
        int a1;
        void fun1();
    public:
        int a2;
        void fun2();
        void set(int b1,int b2,int b3);
        void show();

};

void studen::fun()
{
    cout << a << a1 << a2 << endl;
}

void studen::set(int x,int b,int c)
{
    a = x;
    a1 = b;
    a2 = c;
}

void studen::show()
{
    cout << a << ' ' << a1 << ' ' << a2 << ' '<< endl;
}

int main()
{

    studen s;
    s.set(1,2,3);
    s.show();
    return 0;
}

```

C++的输出

```C++
C++的输入以及输出是以流的形式
    在定义流对象时，系统会在内存开辟一段缓冲区，用来暂存输入输出的数据。
    	在执行输入输出语句时，先把数据顺序存放在缓冲区中，直到缓冲区满或者遇到endl或者'\n' 或者 ends，flush才会打印输出并且清空缓存区
    '\n':换行
	endl:刷新缓冲区并且换行
	flush:刷新缓冲区
ends:插入空字符，表示结束当前输出流，但是并不会刷新缓存区
```



构造函数和析构函数

```C++
构造函数：在类定义对象的时候用于初始化的，构造函数一般使用初始化列表
    和类名相同，无返回值，参数可以有可以没有，可以重载，系统会在生产类的对象的时候自动调用，无需用户操作
析构函数：在类的对象消亡的时候释放资源的
    和类名相同，前面加个~，参数没有，不可重载，可以重写，系统会在类的对象生命周期结束时自动调用，作用是释放系统资源：比如类里面new了内存没有delete；创建了套接字没有关；打开了文件等……
    所以在创建类的对象的时候就有两种方法，带参数和不带参数，如果你设置了多个构造函数，那么可以根据构造函数的参数不同来创建不同的对象
    eg：
    	#include <iostream>
        using namespace std;

        class Point
        {
            public:
            //构造函数，和类名同名，没有返回值，可以有参数可以没有，能够重载
            //系统会在生成类对象的时候自动调用，作用是初始化
            Point();
            Point(float a,float b);
            //析构函数，也和类名同名，前面有一个～，没有返回值，没有参数，不能重载
            //系统会在类的对象生命周期消亡的时候自动调用，作用是释放系统资源
            ~Point();
            void show();

            private:
                float x;
                float y;


        };

        Point::Point()
        {
            cout << "不带参数的构造函数" << endl;
            x = 1.1;
            y = 2.2;
        }

        Point::Point(float a,float b)
        {
            cout << "带参数的构造函数" << endl;
            x = a;
            y = b;
        }

        Point::~Point()
        {
            cout << "析构函数" << endl;
        }

        void Point::show()
        {
            cout << "x = " << x << "y = " << y << endl;
        }

        int main()
        {
            Point p1;
            p1.show();

            Point p2(1.1,11.11);//带参数生成类的对象
            p2.show();


            return 0;
        }

```

C++delete是运算符，并且调用了析构函数

初始化列表：构造函数不在函数体里面对成员变量初始化，而是在外面用:x(1.1),y(2.2)初始化列表



this指针

```
原理：编译的时候对每一个成员函数增加一个this指针，调用的时候传一个对象的本身的指针；
	指针为空指针时：有没有段错误取决于函数内部有没有用this指针访问了其他变量或函数
```

类的静态

```C++
静态变量或者静态函数是属于类的，所以没有this指针，他只有一个，类可直接访问，对象也可访问
通过类名直接访问：Demo::show_static();
通过对象访问：Demo d1; di.show_static();
就是类里面有成员变量或者成员函数声明成了static的；静态变量必须在类的外面去定义：int Demo::aa = 100;

静态函数不能访问非静态函数或其他非静态成员，因为他没有this指针；
非静态函数可以访问静态函数；
    eg:
        #include <iostream>
        using namespace std;

        class Demo
        {
            public:
                Demo();
                ~Demo();
                static void how()
                {
                    cout << num << endl;
                }

            private:
                static int num;
        };

        int Demo::num = 0;
        Demo::Demo()
        {
            num++;
        }

        Demo::~Demo()
        {
            num--;
        }

        int main()
        {
            Demo p;
            //p.how();

            Demo::how();

            return 0;
        }
因为static修饰的变量只有一份，当使用类定义对象时系统就会调用构造函数，所以num++,这样就可以算出一共定义了多少个对象，当然，打印对象值的函数最好也要static，因为你要直接使用类去调用！！
```

const成员函数

~~~c++
意思就是这个函数不能修改成员变量；
    void show()const;
不能修改成员变量，不能调用非const成员函数：因为可能非const成员函数会涉及改变成员变量的值，很好理解，相对来说就可以调用另外一个const成员函数。
    
    在类外面定义的时候要与类里面声明的格式一样加上const；
    如果一个函数没有改变成员变量的值，就应该声明为const成员函数
~~~



const对象

```
const Demo d1;
此时d1被修饰为const常量对象，值不能被改变
也就是说一个这样的对象不能调用可以改变成员变量值的函数，你只能调用const函数
```

const成员变量

```
就是修饰类里面具体的变量
```

友元：

```C++
friend
友元函数：
	一个函数访问一个类的私有成员
	用法：在一个类的public里friend 这个函数
	友元函数没有类的this指针，定义和friend的时候都需要传参
	eg:
		#include <iostream>
        using namespace std;

        class A
        {   
           
            public: 
            	friend void fun(A& cc);//在此处声明友元时，与函数定义一模一样；
                int a = 9;
            private:
                int b = 20;
        };

        void fun(A& a)
        {
            //a.b = 88;
            cout << a.b << endl;

        }
		
        int main()
        {
            A p;
            fun(p);

            return 0;
        }

	
	
	
友元类：
	一个类访问另一个类的私有成员
	eg:
		#include <iostream>
        using namespace std;

        class B;
        class A
        {
            public:
                int a = 10;
            private:
                int b = 20;

        friend B;
        };


        class B
        {
            public:
                int a = 111;
                void show(A& a)
                {
                    cout << "a.b = " << a.b << ' ' << "a.a = " << a.a << endl;
                }
            private:
                int b = 222;
        };

        int main()
        {
            A a;
            B b;
            b.show(a);

            return 0;
        }
	A类访问B类：A类要成为B类的朋友，在B类中声明A类，然后定义两个类的对象，在A类的函数中传参B类
	在传参的时候他找不到，可以使用前向声明
	
友元成员函数：
	一个类的一个成员函数访问另一个类的私有成员
	A类的成员函数访问B类：在B类中声明A类中的某个函数为友元成员函数，friend addr::fun
        eg:
		#include <iostream>
        using namespace std;

        class A;
        class B
        {
            public:
                void fun(A& a);//B的函数访问A，要先向前声明A；
        };
        class A
        {
            public:
                int a = 10;
            private:
                int b = 1001;
                friend void B::fun(A& a);//A声明B的函数可以访问A，这里要一模一样
        };

        void B::fun(A& a)
        {
            cout << "a.a = " << a.a << "a.b = " << a.b << endl;
        }

        int main()
        {
            A a;
            B b;
            b.fun(a);


            return 0;
        }
	
        
        
```

拷贝构造

```C++
用已经存在的对象初始化新的对象
与构造函数一样：
	Demo(const Demo& a):a(a.a),b(b.a);
	意思就是现在用对象a的数据拷贝到新创建的对象(下面的d2)里实现拷贝，且会调用拷贝构造函数
	意思就是定义一个新的对象的时候用原始的对象的数据来初始化我新的对象
	使用方法：
		1.Demo d2(d1);
		2.Demo d2 = d1;
		这两种方式均可，都调用了拷贝构造函数
		d2 = d1//这样就不是拷贝构造，而是赋值！！，因为d2与d1都已存在
		
		当类的对象作为参数传给一个函数的时候也会调用到拷贝构造函数
		void fun(Demo d)
		{
			d.show();
		}
		此时也调用到拷贝构造函数
		
		
		
		一个函数内部定义了一个局部的对象，函数的返回值为该对象，此时也调用了拷贝构造函数
		-fno-elide-constructors
		
		当用户不自己写拷贝构造函数时系统自动有个叫做浅的拷贝构造函数仅复制
		
		
		为什么要自己写拷贝构造函数？
			因为系统的浅拷贝是字节拷贝，若涉及到new申请空间，那么当第一个对象生命周期结束时会调用析构函数去释放资源，而由于第二个是第一个的拷贝构造函数，所以又会释放相同的空间从而造成double free 段错误
		
            赋值：就相当于系统自己的拷贝构造函数，也会出现double free
```

```
构造函数  析构函数 拷贝构造函数 赋值运算符 地址运算符
构造函数：初始化
析构函数：释放资源
拷贝构造函数：已有的对象初始化新对象  类对象作为函数的形参  类对象作为函数的返回值
赋值运算符：系统的是浅拷贝，也会出现double free
```

运算符重载

```c++
为什么要重载运算符？
	因为两个用户自定义的数据类型想要进行运算；
	运算符的重载就是函数的重载(因为是类似函数形式实现的)
	
友元运算符重载：(可在外部实现，也可直接在内部实现)
    
friend 类型 operator运算符(a,b);
friend Integer operator + (Integer& i1,Integer& i2);
最好加上const避免误操作修改了值
friend Integer operator + (const Integer& i1,const Integer& i2);
eg:
	Integer operator+(const Integer& i1,const Integer& i2)
      {
          Integer t(i1.a+i2.a);
          return t;
      }
      
      operator++(Integer a);
      operator++(Integer a,int a);
      第一个是前++ 第二个是后++，其中的a只是一个标志位，表示他是后++，实际没有操作
注意：构造函数要定义，因为你相当于是在函数内部定义一个新的临时类的对象，使用的是构造函数去初始化这个新的对象，不定义构造函数会err

特点：运算符的重载实质是函数重载，没有this指针，双目运算符来说，需要两个参数；单目运算符来说需要一个参数；



成员运算符重载：(成员函数)
	class Point
    {
        private:
        double x;
        double y;
        
        public:
        Point(double a = 0,double b = 0):x(a),y(b){}
		
        void set(double a,double b)
        {
            x = a;
            y = b;
        }
        
        Point operator+(const Point& p);
        
        //只有一个参数：前++
        Point& operator++();
        //多一个参数：后++
        Point operator++(int a);
		Point& operator-=(const Point& p);        
      	Point& operator==(const Point& p);
        
        
    };

return *this;
	表示返回自身；如果函数的返回值设置成引用就代表返回自身的引用
        () [] -> = ：只能是成员函数重载，只能放在一个类里面，不能是全局函数的形式

```

模板：

```c++
模板只能定义在全局或者名字空间内：
    模板有函数模板和类模板
    
    函数模板：
    	template<typename T>
    	template<class T>//都可以
    	T add(T a,T b)
		{
			return a+b;    
		}
    T:待定的数据类型
        int a = 100,b = 200,c;
        c = add(a,b);


    自定义数据类型
        class Integer
        {
            public:
                Integer(int a = 0):aa(a){}
            private:
            int aa;
        };

        Integer i1(10),i2(20);
        cout << add(i1,i2) << endl;

        运算符重载，cout <<重载



           模板函数作用在用户自定义类上的时候，需要重载运算符，重载函数内用到的所有的运算符，＋就重载+，然后才能调用模板函数 


            模板类里面的成员函数使用模板，模板成员函数在外面实现情况：
            template<typename T>
            class A
            {
                public:
                    A(T a):aa(a){}
                    void show();
                private:
                    T aa;
            };

            template<typename T>
            void A<T>::show()//当模板的成员函数在外面实现的时候形式
            {
                cout << aa << endl;
            }


模板类：(ALL)--------------------------------------------------------------------------------

        #include <iostream>
        using namespace std;

        template<typename T,typename T1>
        class A
        {
            public:
                A(T a,T1 b):aa(a),bb(b){}
                void show();

                template<typename G>
                    void show2(G a)
                    {
                        cout << aa << ' ' << bb << endl;
                    }

            private:
                T aa;
                T1 bb;
        };
        template<typename T,typename T1>
        void A<T,T1> :: show()
        {
            cout << aa << ' ' << bb << endl;
        }


        int main()
        {
            //模板类实例化对象
            A <int,float>p(23,4.4444);
            p.show();
            p.show2(3);


            return 0;
        }



模板类：(类使用模板后重载)
    



```

派生和类继承

```C++
派生：
    一个类的基础上派生出来的新类，具有基类的所有数据信息

继承方式：
    
    基类的private属性的成员在派生类始终都是不可见的，无论哪种继承方式
    public:
		原基类是啥属性，派生类对于基类的所有成员也是啥属性
	protected:
		原基类是啥属性，派生类对于基类的所有成员变成protected
	private:
		原基类是啥属性，派生类对于基类的所有成员变成private

            后俩都不能通过派生类的对象直接访问
名字遮盖问题：
    当派生类的成员名与基类重名时：通过派生类的对象访问不到基类的成员，因为派生类有同名成员，派生类的对象就访问不到基类的成员；
     d1.Base::aa = 100;
	只能用以上加作用域的方式去访问基类的成员
	
   构造与析构都不能被继承     
构造函数：
	先把派生自己的成员初始化，然后基类的类名加参数
        Derived(int a,int b,float c):a(a),Base(b,c);

基类先构造，派生先析构
    
    类型一：
    
    多继承：
    	class A
        {
            public:
            	A(int a):aa(a)
                {
                    cout << "A构造" << endl;
                }
            	~A()
                {
                    cout << "A析构" << endl;
                }
            	
            	void showA()
                {
                    cout << "aa = " << aa <<endl;
                }
            protected:
            	int aa;         
        };


    	class B
        {
            public:
            	B(int a):bb(a)
                {
                    cout << "B构造" << endl;
                }
            	~B()
                {
                    cout << "B析构" << endl;
                }
            	
            	void showB()
                {
                    cout << "bb = " << bb <<endl;
                }
            protected:
            	int bb;         
        };

 	class C
        {
            public:
            	C(int a):cc(a)
                {
                    cout << "C构造" << endl;
                }
            	~C()
                {
                    cout << "C析构" << endl;
                }
            	
            	void showC()
                {
                    cout << "cc = " << cc <<endl;
                }
            protected:
            	int cc;         
        };


		class Derived:public C,protected B,private A
        {
            public:
            	Derived(int a,int b,int c,int d):
            		dd(d),A(a),B(b),C(c)
                    {
                        cout << "Derived构造" << endl;
                    }
            	~Derived()
                {
                    cout << "Derived析构" << endl;
                }
            
            	void show()
                {
                    cout "aa = " << aa << "bb = " << bb << "cc = " << cc << "dd = " << dd << endl;
                }
            private:
            	int dd;
        };


构造函数的调用顺序和继承顺序一致，析构函数则反之
    
    基类构造，成员变量构造(有一个类的对象是派生的成员变量)，派生类自己构造
    基类：继承时顺序
    成员变量：定义的顺序
    
    
    	
    
    
    类型二：
    	class A
        {
            public:
            	A(int a):aa(a)
                {
                    cout << "A的构造" << endl;
                }
            	~A()
                {
                    cout << "A的析构" << endl;
                }
            
            protected:
             int aa;
        };

    	class B:public A
        {
            public:
            	B(int a,int b):bb(b),A(a)
                {
                    cout << "B的构造" << endl;
                }
            	~B()
                {
                    cout << "B的析构" << endl;
                }
            
            protected:
             int bb;
        };
        
		class Derived:public B
        {
            public:
            	Derived(int a,int b,int d):dd(d),B(a,b)
                {
                    cout << "Derived构造" << endl;
                }
                ~Derived()
                {
                    cout << "Derived析构" << endl;
                }
            	void show()
                {
                    cout << "aa = " << aa << "bb = " << bb << "dd = " << dd << endl;
                }
            private:
            	int d;
        };
            
            构造顺序：A B Derived
            析构顺序：相反
            
            
    类型三：(菱形继承)
    	class A
        {
            public:
            	A(int a):aa(a)
                {
                    cout << "A构造 aa = " << aa << endl;
                }
            	~A()
                {
                    cout << "A析构" << endl;
                }
            protected:
            	int aa;
        };
            
         class B1:virtual public A
         {
             public:
             	B1(int b,int a):b1(b),A(a)
                {
                    cout << "B1构造 b1 = " << b1 << endl;
                }
             	~B1()
                {
                    cout << "B1析构" << endl;
                }
             protected:
             	int b1;
         };
            
            
         class B2:virtual public A
         {
             public:
             	B2(int b,int a):b2(b),A(a)
                {
                    cout << "B2构造 b2 = " << b2 << endl;
                }
             	~B2()
                {
                    cout << "B2析构" << endl;
                }
             protected:
             	int b2;
         };            
            
          class Derived:public B1,public B2
          {
              public:
              	Derived(int a,int b1,int b2,int d):dd(d),B1(b1,a),B2(b2,a),A(a)//这里参数的顺序要和基类的传参顺序一样
                {
                    cout << "Derived构造 dd = " << dd << endl;
                }
                ~Derived()
                {
                    cout << "Derived析构" << endl;
                }
              
              	void show()
                {
                    cout << "aa = " << aa << "b1 = " << b1 << "b2 = " << b2 << "dd" << endl;
                }
              private:
              	int dd;
          };




虚基类：virtual,在派生的时候加上这个关键字，表示基类为虚基类，不管有几条路，到最后的派生类都只拷贝一份，避免数据冗余和数据冲突,此方式叫做虚继承

    在最后的派生类的构造函数时：不仅需要完成自己以及直接基类的构造，也要完成最顶层的虚基类的构造，意思就是再加一个虚基类的构造
            

```

向上转型

```C++
派生类赋值给基类：
	包括值，对象，指针，引用
	
	class Base
	{
        public:
        	Base(int a):b1(a){}
        	void show()
            {
                cout << "Base::show() b1 = " << b1 << endl;
            }
		//protected:
        	int b1;
	};

	class Derived:public Base
    {
       public:
       	  Derived(int a,int d):d1(d),Base(a){}
        void show()
        {
            cout << "Derived::show() b1 = " << b1 << "d1 = " << d1 << endl;
        }
        int d1;
    };

	main()
    {
        Base b1(1);
        b1.show();
        
        cout << "---------------------" << endl;
        
        Derived d1(10,11);
        d1.show();
        
        //派生类赋值给基类
        b1 = d1;
        b1.show();
          
        //指针
        Base *pBase;
        pBase = &d1;//基类的指针被派生类的对象赋值
        pBase.show();//调用基类的函数
        
        //引用
        Base& ref_Base = d1;
        ref_Base.show();
        
        
        以上三种结果均相同：使用函数为基类，使用数据为派生类；
    }


继承和组合：均能实现某些功能的叠加
    

    class Phone
    {
        public:
        void make_call(string name)
        {
            cout << "打电话给" << name << endl;
        }
    };

	//以继承方式实现
	class Car1:public Phone
    {
        public:
        	void drive()
            {
                cout << owner_name << "正在开车" << endl;
            }
        string owner_name;
    };
	
	Car1 c1;
	c1.owner_name = "李二狗";
	c1.drive();
	c1.make_call("yzy");



	//以组合方式
	class Car2
    {
        public:
        	Phone ph;
        	void drive()
            {
                cout << owner_name << "正在开车" << endl;
            }
        	string owner_name;
    };

	
```

大小端

```
小端：低字节放在低地址
大端：高字节放在低地址
```

多态：

​	何为多态？？？

​		需要借助于虚函数才能实现多态：基类指针指向基类的对象就使用基类的成员（包括成员变量和成员函数），基类指针指向派生类的对象就使用派生类的成员（包括成员变量和成员函数）；

​		再简单的换句话来说：基类的指针可以按照基类的方式做事，也可以按照派生类的方式来做事，它具有多种形态，多种表现形式，我们称这种现象叫做多态（Polymorphism）

```C++
多态：
    class Company
    {
        public:
        Company(string name):company_name(name){}
        //将slogan函数声明为虚函数
        virtual void slogan()
        {
            cout << company_name << ":it`s a great company" << endl;
        }
        protected:
        string company_name;
    };


	class Nike:public Company
    {
        public:
        Nike(string name):Company(name){}
        
        void slogan()
        {
            cout << company_name << ":Just do it!" << endl;
        }
    };


	class NikeChina:public Nike
    {
        public:
        	NikeChina(string name):Nike(name){}
        void slogan()
        {
            cout << company_name << ": G0,Beijing!" << endl;
        }
    };

	//使用一个同一接口调用不同对象的slogan函数
	void saySlogan(Company& c)
    {
        c.slogan();
    }


	main()
    {
        //三个类的对象
        Company company("有限公司");
        Nike nike("耐克");
        NikeChina nikeChina("中国耐克");
        
        //分别调用三个函数
        company.slogan();
        nike.slogan();
        nikeChina.slogan();
        
        //使用统一接口调
        saySlogan(company);
        saySlogan(nike);
        saySlogan(nikeChina);
        这里的输出结果会出现公司名字对，但是slogan不对，原因是向上转型后，数据是派生类的，但是调用的函数是基类的
        需要将基类的slogan函数声明为虚函数，相应的派生类的同名的函数自动成为虚函数，需要派生类的函数与基类的函数一模一样，包括函数名 返回值 形参
            
            
        return 0;
    }

什么叫虚函数？
    在类的里面用virtual修饰的函数

    virtual int add(int a,int b)
	{
    	return a+b;
	}

此操作错误！！！！类外面的一个全局函数不能声明为虚函数
    
    class Demo
    {
        public:
        	virtual void show()
            {
                cout "虚函数" << endl;
            }
    };
	静态函数不能称为虚函数
    类外面的函数不能声明为虚函数
    构造函数不能声明为虚函数
    析构函数可以是虚函数！！！！！，且一般情况下应该把析构函数定义为虚函数！！


        
    派生类重写(覆盖)基类的虚函数：重写
        
        
        
    重载：
    class Demo
    {
        public:
        	Demo(int a):aa(a){}
        	void func1(int a,int b)
            {
                cout << "Demo::func1(int a,int b)" << endl;
            }
        
        	void func1(int a,float b)
            {
                cout << "Demo::func1(int a, float b)" << endl;
            }
        
        	int& getValue()
            {
                return aa;
            }
        	
        	int& getValue() const
            {
                return aa;
            }
        	//此时可以构成函数的重载，因为const也算
        在调用的时候要看你是否改变了其他变量的值，因为从const只能调用const变量或函数
        private:
        	int aa;
    };
        
        

	main()
    {
        const Demo d2(100);
        cout << d2.getValue() << endl;
        //结果为调用const的getValue，因为此时的对象d2是const只读的，不允许改变值
        
        
        return 0;
    }


	
	
	隐藏：
        class Base
        {
            public:
            	void func1(int a)
                {
                    cout << "Base::fun1(int a)" << endl;
                }
            
            	void func1(float a)
                {
                    cout << "Base::fun1(float a)" << endl;
                }
            	void fun2(int a,int b)
                {
                    cout << "Base::fun2(int a,int b)" << endl;
                }
            	//虚函数
            	virtual void fun3(int a)
                {
                    cout << "Base::fun3(int a)" << endl;
                }
            
        };

    	class Derived:public Base
        {
            public:
            	void func1()
                {
                    cout << "Derive::func1()" << endl;
                }
            //此时只要函数名一样，无论参数，基类没有声明virtual，都隐藏基类的函数
      			
            
            	void fun2(int a,int b)
                {
                    cout << "Derived::fun2(int a,int b)" << endl;
                }
            //参数完全一样，基类没有声明virtual，则隐藏基类的函数
            
            	void fun3(int a)
                {
                    cout << "Derived::fun3(int a)" << endl;
                }
            //虽然派生类没有关键字virtual，但是因为基类声明了，所以这里也是虚函数
            //此时一模一样，但是由于基类声明了此函数为虚函数，所以是覆盖(重写)了基类中完全相同的虚函数
        };

	





	只要函数一模一样，且是虚函数，就叫覆盖；其余情况都叫隐藏，隐藏和覆盖都是调用派生类的函数

    这个东西就是想讲虚函数的作用，只有覆盖是有虚函数的，所以就是实现覆盖的例子
    虚函数：
    	class Person
        {
            public:
            	Person(string name,int age):name(name),age(age){}
            
            	void who()
                {
                    cout << "我是" << name << " 今年" << age << "岁" << endl;
                }
            	virtual void work()
                {
                    cout << "暂时没有工作" << endl;
                }
            	virtual void hobby()
                {
                    cout << "没啥爱好" << endl;
                }
            
            protected:
            	string name;
            	int age;
        };
    
    	class Teacher:public Person
        {
            public:
            	Teacher(float a,string name,int age):score(a),Preson(name,age){}
            	void work()
                {
                    cout << "我的工作是一名老师" << endl;
                }
                void hobby()
                {
                    cout << "我喜欢打篮球" << endl;
                }
            private:
            	
            float score;
        };
    
		//使用一个统一的接口去调用
		void show(Person& p)
        {
            p.who();
            p.work();
            p.hobby();
        }

		main()
        {
            //向上转型
            Person *p1 = new Worker("张三",32,80);
            p1->who();
            p1->work();
            p1->hobby();
               
        }



	虚函数实现原理：
        class A1
        {
            public:
            	long aa;
            	void func1(){}
        };
        
        class A2
        {
            public:
            	long aa;
            	virtual void func1(){}
        };
        
        
        main()
        {
            A1 a1;
            A2 a2;
            
            cout << "a1 = " << sizeof(a1) << endl;
            cout << "a1 = " << sizeof(a2) << endl;           0
            return 0;
        }
        
        含有虚函数的类叫多态类
        
        
        
        多继承：
            class A1
            {
                public:
                	virtual void func1(){}
            };
        
			class A2
            {
                public:
                	virtual void func2(){}
            };
        
        
        	class Derived:public A1,public A2
            {
                public:
                	long aa;
                
            };
        
			main()
            {
                cout << sizeof(Derived) << endl;
                //此时继承了两个多态类，多了两个指针 24
            }
        
        
        对象一个虚指针一个，一共一个虚函数表，派生类都会继承基类的虚函数表
    
            
            
            纯虚函数：
            	//含有纯虚函数的类叫抽象类
            	//不能实例化
            	class Shape
                {
                   public:
                    	//定义纯虚函数
                    	//可以把纯虚函数当作一个接口，需要在派生类里面实现
                   		virtual double circurference() = 0;
                    	virtual double area() = 0;
                };

				class Circle:public Shape
                {
                    public:
                    	Circle(double r):radius(r){}
                    
                    	virtual double circurference()
                        {
                            return 2*3.14*radius;
                        }
                    
                    	virtual double area()
                        {
                            return 3.14*(radius*radius);
                        }
                    	void show()
                        {
                            cout << "circurference is " << circurference() << endl;
                            cout << "area is " << area();
                        }
                    private:
                    	double radius;//半径
                };
		
				main()
                {
                    Shape s;//err
                    //不能实例化对象
                    
                    Circle cr(10);
                    cr.show();
                    
                    
                    return 0;
                }

	在派生类里面必须把抽象基类所有的纯虚函数都重写(覆盖)才能使用派生类实例化对象；
	意思就是基类有多少个纯虚函数，在派生类里都应该实现，就是实现纯虚函数
```

虚析构函数

```C++

	析构一般声明为虚函数:意思就是将派生类的对象赋值给基类的指针，去向上转型，若此时派生类涉及到了内存申请，析构函数不是虚函数的话就无法彻底释放内存空间
        class Base 
        {
            public:
            	Base()
                {
                    cout << "Base构造" << endl;
                }
            	~Base()
                {
                    cout << "Base析构" << endl;
                }
        };


        class Derived:public Base 
        {
            public:
            	Derived(int len):len(len),p(nullptr)
                {
                    cout << "Derived构造" << endl;
                    p = new int[len];
                }
            	~Derived()
                {
                    cout << "Derived析构" << endl;
                    if(p != nullptr)
                    {
                        delete []p;
                        p = nullptr;
                    }
                }
            private:
            	int len;
            	int *p;
        };


		main()
        {
            Derived d1(100);
            
            Base *p = new Derived(10);
            delete p;//此时只能释放Base的空间，只能调到基类的析构
            //需要声明析构为虚函数
            
            return 0;
        }
```

关于隐藏和重写(覆盖)

```C++
        #include <iostream>
        using namespace std;
        
        class A
        {
            public:
                void show()//隐藏的时候不加virtual
                //覆盖的时候加virtual
                {
                    cout << "类A的函数" << endl;
                }
        };

        class B:public A
        {
            public:
                void show()
                {
                    cout << "类B的函数" << endl;
                }
        };

        int main()
        {
            B b;
            b.show();//此操作为隐藏
----------------------------------------------
            B bb;
            A *p = &bb;
            p->show();//此操作为覆盖

            return 0;
        }
意思就是：
	当我直接调用派生类的函数时，由于是隐藏，所以调用的是派生类的函数；
	当我向上转型调用基类的函数时，由于是覆盖，派生类覆盖了基类的函数，所以调用的也是派生类的函数；
	两个show函数均为派生类函数
```





day8

```C++
异常：处理运行时出错的机制
    exception
    
    main()
	{
    
    try//检测有可能发生运行时错误的代码
    {
        cout << "try 1" << endl;
        int aa = 100;
        throw (aa);//异常、错误发生的时候抛出异常，
        		   //就是检测到错误了，但是不在当前代码处理，在catch处捕获处理
        
        cout << "try 2" << endl;//此语句不会被执行，异常发生后的代码不会被执行
    }
	catch(int& e)//捕获异常，进行处理
    {
        cout << "异常的内容是:" << e << endl;
    }	
    /*
    catch(double & e)//没有捕获
    {
        cout << "异常的内容是:" << e << endl;
    }
    */
		cout << "about to end thr programme" << endl;
}

若抛出了异常而没有捕获，系统将捕获该异常，终止当前程序执行
    
    
    系统标准异常：
    #include <excepction>
    
    main()
	{
  		string s1 = "this is a big world";
    	try
        {
        	cout << "try 1" << endl;
            //char c1 = s1[100];//没有抛出异常
            char c1 = s1.at(100);//抛出异常
            cout << "c1 = " << c1 << endl;
            cout << "try 2" << endl;
        }
    
    	catch(exception& e)//尝试捕获系统自定义异常
        {
            cout << e.what() << endl;//what函数
        }
    
    
    
    异常发生的位置不在当前try代码段：
        void func1()
    	{
        	cout << "func1 1" << endl;
        	func2();
        	cout << "func1 2" << endl;
    	}
    
    	void func2()
        {
            cout << "func2 1" << endl;
            int aa = 1000;
            throw(aa);
            cout << "func2 2" << endl;
        }
    
    
    	main()
        {
            try
            {
                cout << "try 1"  << endl;
                func1();
                cout << "try 2" << endl;
            }
            catch(const int& e)
            {
                cout << "excepcton info "<< endl;
            }
        }
    
    只要异常后面的代码语句都不被执行
    
        
        ----多级catch----
        因为一个try里面可能有多个异常，如果你只捕获一次其他的没捕获就会导致程序中止
        
        
        向上转型 
        class Base
        {
            public:
            	Base(int i):errCode(i){}
            	virtual void show_error()
                {
                    cout << "错误码: " << errCode << endl;
                }
            
            protected:
            	int errCode;
        };
        
        class Derived:public Base
        {
            public:
            	Derived(int i,string info):errInfo(info),Base(i){}
            	void show_error()
                {
                    cout << "错误码: " << errCode << endl;
                    cout << "错误信息:" << errInfo << endl;
                }
            
            protected:
            	int errCode;
            	string errInfo;
        };
        
        void func1(int type)
    	{
        	if(type == 0)
            {
                int aa = 100;
                throw(aa);
            }
    	
    		else if(type == 1)
            {
                const char* p = "hello";
                throw(p);
            }
            
            else if(type == 2)
            {
                Base b1(1001);
                throw(b1);
            }
            else if(type == 3)
            {
                Derived d1(1003,"the index out of range");
                throw(d1);
            }
    	}
        
    	main()
        {
            try
            {
                //有可能抛出多种异常，所以需要多级的捕获catch
                func1(3);    
                //此时为3，虽然没有接收，因为多态
            }
            catch(const int& e)
            {
                cout << "异常信息:" << e <<endl;
            }
            catch (const char* p)
            {
                cout << p << endl;
            }
            catch(Base& b)//2和3都能处理，向上转型
            {
                b.show_error();
            }
            
            return 0;
        }
    
    没有捕获完全：terminate系统调用函数结束程序

        
        throw：异常规范，表示一个函数可能会抛出什么异常
        
        
        使用标准异常：
        
        
    #include <iostream>
    #include <exception>
 	using namespace std;
    int g_array[100];

    int& getArrayData(int index)
    {
        if(index >= 0 && index <100)
            return g_array[index];
        else
        {
            std::out_of_range e("数组越界");//exception类的派生类out_of_range
            throw(e);
        }
    }

    float fun(float a)
    {
        float ret = 10 / a;
        if(a == 0)
        {
            std::invalid_argument e("除数为0");
            throw(e);
        }
        return ret;
    }


    int main()
    {
        try
        {
            //cout << getArrayData(101) << endl;
            fun(0);
        }

        catch(out_of_range& e)
        {
            cout << e.what() << endl;
        }
        catch(invalid_argument& e)
        {
            cout << e.what() << endl;
        }
        //由于是基类，所以直接用向上转型也行
        catch(exception& e)
        {
            cout << e.what() << endl;
        }

        return 0;
    }
        what函数是虚函数，由于是向上转型，所以调用的也是派生类的函数：覆盖
            
            
            
            自己派生(定义)一个异常类：
            #include <exception>
            
            class MyException:public exception
            {
                public:
                	MyException(const char *p):info(p){}
                	
                	virtual const char* what() const noexcept//加不加virtual都一样
                    {
                        return info;
                    }
                private:
                	const char* info;
            };
            
            float fun(float a)
            {
                if(a == 0)
                {
                    MyException e("除数为0");
                    throw(e);
                }
                float ret = 10 / a;
                return ret;
            }
    
    		main()
            {
                try
                {
					fun(0);
                    
                }
                catch(exception& e)
                {
                    cout << e.what() << endl;
                }
            }
    
    
    创建自己的标准异常类：
         	class NewException
            {
                public:
                	NewException(int code,string info)
                        :errCode(code),errInfo(info){}

             		void show()
                    {
                        cout << "错误码:" << errCode << "错误信息:" << errInfo << endl;
                    }
                private:
                	int errCode;
             		string errInfo;
            };
            
        
        	float fun(float a)
            {
                if(a == 0)
                {
                    NewException e(1001,"除数为0");
                    throw(e);
                }
                float ret = 10 / a;
                return ret;
            }
        
            main()
            {
                 try
                    {
                        fun(0);

                    }
                    catch(NewException& e)
                    {
                        e.show();
                    }
            }

        就相当于用了一个用户自定义的数据类型---自定义类的对象去throw，但是捕获的时候也只能用该用户定义的类的引用捕获
        
        
            
            
            1.当我使用throw主动的抛出异常的时候，不能用标准异常类的对象去捕获，只能用对应数据类型去捕获
        	2.只有使用标准异常类的派生类throw一个异常的时候才能用基类exception的对象去捕获，相当于向上转型，因为使用到的是类里面的一个what函数，而且它是一个虚函数，所以构成了覆盖
            函数原型：virtual const char* what()const noexcept;
    		3.自己派生一个基于exception的异常类，catch的时候也用exception类的捕获
            4.自己定义一个标准异常类，就不能用exception捕获了
            5.注意，不可用基类带参数的实例化一个对象，因为基类长这个样子：
                   C++ exception类：C++标准异常的基类
                    class exception {
                    public:
                      exception () noexcept; //构造函数
                      exception (const exception&) noexcept; //拷贝构造函数
                      exception& operator= (const exception&) noexcept; //运算符重载
                      virtual ~exception(); //虚析构函数
                      virtual const char* what() const noexcept;
                    }		
                    在代码中可以直接使用标准异常类（invalid_argument, out_of_range）

                   意思就是你不能直接用exception去实例化一个：exception e("抛出了一个异常");此操作错误，你只能使用该类去实例化一个对象，但是不能带参数
                
                
```

强制类型转换

~~~c++
四种：
    编译期间：
    	static_cast
    		基本数据类型之间
    
    		class Base
            {
                public:
                	Base(int b):b1(b){}
                
                	void show()
                    {
                        cout << "Base::b1 = " << b1 << endl;
                    }
                protected:
                	int b1;
            };

			class Derived:public Base
            {
                public:
                	Derived(int b,int d):d1(d),Base(b){}
                
                	void show()
                    {
                        cout << "Derived::b1" << b1 << "Derived::d1" << d1 << endl;
                    }
                protected:
                	int d1;
            };  
    		main()
			{
    
    			float f1 = 3.14;
    			cout << "f1 = " << f1 << ednl;
				
    			int i1 = static_cast<int>f1;
    			cout << "i1 = " << i1 << endl;

				int i2 = 22;
    			i1 = 27;
    			char c1 = static_cast<char>(i1 + i2);
				cout << "c1 = " << endl;    

                ------------------------------------------------
                
                Base b1(100),*pB1 = nullptr;
                Derived d1(1000,2000),*pD1 = nullptr;
                
                //向上转换 upcast
                pB1 = &d1;//向上转型,因为此处没有覆盖，所以调用基类函数
                pB1->show();//安全
                
                //向下转换 downcast
                //pD1 = &b1;//err
                pD1 = ststic_cast<Derived *>(&b1);//强转 不安全
                pD1->show();//此时b1 = 100，d1 = 随机数，因为Base类没有d1，数据不够，所以不安全
                //向上转换 引用
                Base& base_ref = d1;
                base_ref.show();
                
                //向下转换 引用
                //Derived& derived_ref = b1;//err
                Derived& derived_ref = static_cast<Derived&>(b1);
                derived_ref.show();
			}


static_cast:	
	基本数据类型转换，基类与派生类转换，向下转换时需要用到此强转；
 -------------------------------------------------------------------------------   
    
    
    	reinterpret_cast 程序员自己保证安全性
            特别灵活，特别危险
     		          
     基础类型-指针  基础类型
     
            int main()
 			{
				long l1 = 0x31323334353637;     
     			cout << "l1 = " << l1 << endl;
     			char *pChar = reinterpret_cast<char *>(&l1);
                //将长整型转换为字符指针
     			cout << "pChar = " << pChar << endl;                 
     			结果：7654321
 			}



			情况2：自定义类
             class Demo
             {
                 public:
                 	Demo(int a,int b):aa(a),bb(b){}
                 
                 	void show()
                    {
                        cout << "Demo::aa = " << aa << "Demo::bb = " << bb<< endl;
                    }
                 private:
                 	int aa;
                 	int bb;
             };

			main()
            {
                Demo d1(100,200);
                int i1 = 300;
                d1.show();
                //将类的对象的数据类型转换为int型
                int *p = reinterpret_cast<int *> (&d1);
                cout << "*p" = << *p << endl;
                //结果输出100
                
                Demo *pDemo = reinterpret_cast<Demo *>(&i1);
                pDemo->show();
                
                //将一个指针转化为数据类型
                char c1 = 'A';
                l1 = reinterpret_cast<long>(&c1);
				cout << "l1 = " << l1 << endl;
                //输出地址
                
                //将一个数据类型转化为指针
                pChar = reinterpret_cast<char *>(i1);
                cout << "*pChar = " << *pChar << endl;
                //输出A本身
                
            }
reinterpret_cast:
	用于将一个数据的地址转换为另一种数据类型；用于将一个数据的地址转换为另一种数据类型的地址；意思是转换的必须是地址值才行，可以把数据的地址值转换为一个数据，也可将该数据转换为地址值
        也就是可以将某数据类型的地址转换为另一种地址，也可转换为另一种数据类型！！！
--------------------------------------------------------------------------------
    	const_cast
            去除const属性
            	void func(const int& ref)
        		{
            		//去掉了变量的const属性
            		int& tmp = const_cast<int&>(ref);
            		tmp++;                      
        		}            
            	main()
                {
                    int aa = 100;
                    cout << "aa = " << aa << endl;
                    
                    func(aa);
                    cout << "aa = " << aa << endl;               
                    return 0;
                }
            
const_cast:
	用于去掉该变量的const属性，但是，如果你定义了一个const型的变量，再用一个引用或指针去掉他的const属性，改变引用或指针的值，该变量值还是不能改变，只是说语法没错而已；
            
 -----------------------------------------------------------------------------           
                                            
    运行期间:用于继承，多态继承里，也就是继承关系中必须是多态类的继承
		dynamic_cast
        
        class A
        {
            public:
            	A(int a):a1(a){}
            
            	virtual void show()
                {
                    cout << "A::a1 = " << a1 << endl;
                }
            protected:
            	int a1;
        };
        


		class B:public A
        {
            public:
            	B(int a,int b):b1(b),A(a){}
            
            	virtual void show()
                {
                    cout << "B::a1 = " << a1 << "B::b1 = " << b1 << endl;
                }
            protected:
            	int b1;
        };
        
        
		class C:public B
        {
            public:
            	C(int a,int b,int c):b1(b),B(a,b){}
            
            	virtual void show()
                {
                    cout << "C::a1 = " << a1 << "C::b1 = " << b1
                        << "C::c1 = " << c1 << endl;
                }
            protected:
            	int c1;
        };


		main()
        {
            A a1(1);
            B b1(10,20);
            C c1(100,200,300);
            
            a1.show();
            b1.show();
            c1.show();
            
            //
            A *pTmp = nullptr,*pA = nullptr;
            B *pB = nullptr;
            C *pC = nullptr;
            
            //向上转型
            pTmp = &c1;
            //向上转型 安全
            pA = dynamic_cast<A *>(pTmp);
            if(pA != nullptr)
            {
                pA->show();
            }
            else
            {
                cout << "dynamic转换失败" << endl;
            }
            
            //upcast
            pB = dynamic_cast<B*>(pTmp);
            if (pB != nullptr)
            {
                pB->show();
            }
            else
            {
                cout << "dynamic cast failed" << endl;
            }

            //peer cast 平级的转换
            pC = dynamic_cast<C*>(pTmp);
            if (pC != nullptr)
            {
                pC->show();
            }
            else
            {
                cout << "dynamic cast failed" << endl;
            }

            print(2);
            //upcast
            pTmp = &b1;
            //upcast, safe
            pA = dynamic_cast<A*>(pTmp);
            if (pA != nullptr)
            {
                pA->show();
            }
            else
            {
                cout << "dynamic cast failed 1" << endl;
            }

            //peer cast
            pB = dynamic_cast<B*>(pTmp);
            if (pB != nullptr)
            {
                pB->show();
            }
            else
            {
                cout << "dynamic cast failed" << endl;
            }

            //downcast 
            pC = dynamic_cast<C*>(pTmp);
            if (pC != nullptr)
            {
                pC->show();
            }
            else
            {
                cout << "dynamic cast failed" << endl;
            }


            print(3);
            //upcast
            pTmp = &a1;
            //peer cast, safe
            pA = dynamic_cast<A*>(pTmp);
            if (pA != nullptr)
            {
                pA->show();
            }
            else
            {
                cout << "dynamic cast failed 1" << endl;
            }

            //downcast, unsafe
            pB = dynamic_cast<B*>(pTmp);
            if (pB != nullptr)
            {
                pB->show();
            }
            else
            {
                cout << "dynamic cast failed" << endl;
            }

            //downcast, unsafe
            pC = dynamic_cast<C*>(pTmp);
            if (pC != nullptr)
            {
                pC->show();
            }
            else
            {
                cout << "dynamic cast failed" << endl;
            }
            
            
        }

dynamic_cast:
	用于类继承关系中的，且是虚继承，虚基类，涉及到多态的类继承
    向上转型和static是一样的效果，不能向下转换！！！！！！！！！只是dynamic更为安全，因为转型失败的话是nullptr，因为他是两个类之间的转型，只能是指针或者引用，引用失败也是返回nullptr



         
        
  总结归纳：
        static_cast:	
	基本数据类型转换，基类与派生类转换，向下转换时需要用到此强转；
        reinterpret_cast:
	用于将一个数据的地址转换为另一种数据类型；用于将一个数据的地址转换为另一种数据类型的地址；意思是转换的必须是地址值才行，可以把数据的地址值转换为一个数据，也可将该数据转换为地址值、
        用于将一个整型数据转换成指针或引用；转换成地址
        也就是可以将某数据类型的地址转换为另一种地址，也可转换为另一种数据类型！！！
        const_cast:
	用于去掉该变量的const属性，但是，如果你定义了一个const型的变量，再用一个引用或指针去掉他的const属性，改变引用或指针的值，该变量值还是不能改变，只是说语法没错而已；
        dynamic_cast:
	用于类继承关系中的，且是虚继承，虚基类，涉及到多态的类继承，且不能用于基础数据类型，只能用于多态中！！！！！！！！！
    向上转型和static是一样的效果，不能向下转换！！！！！！！！！只是dynamic更为安全，因为转型失败的话是nullptr，因为他是两个类之间的转型，只能是指针或者引用，引用失败也是返回nullptr

~~~

智能指针

~~~c++
	例：在一个函数内多次开辟了内存空间，但是退出的时候没有释放，智能指针就能自动释放内存空间
  	作用：防止内存泄漏
	使用智能指针需要包含头文件
    引用计数器
    #include <memory>

	shared point:共享智能指针，多个智能指针同时拥有一块内存
    
    class Array
    {
        public:
        	Array(int len):len(len),p(nullptr)
            {
                cout << "Array构造" << endl;
                if(len > 0)
                {
                    p = new int[len];
                    for(int i = 0;i < len;i++)
                    {
                        p[i] = i+100;
                    }
                }
            }
        		
        	~Array()
            {
                cout << "Array析构" << endl;
                if(p != nullptr)
                {
                    delete []p;
                    p = nullptr;
                }
            }
        
        	void show()
            {
                for(int i = 0;i < len;i++)
                {
                    cout << p[i] << ' ';
                }
                cout << endl;
            }
        private:
        	int len;
        	int *p;
    };
    
	void func()
    {
        //智能指针是一个模板类
        //方式一
        shared_ptr<Array> p1(new Array(100));//模板类的使用<>
        
        //方式二
        //make_shared是一个模板函数,它负责对智能指针分配内存
        shared_ptr<Array> p1 = make_shared<Array>(100);
        
        //智能指针重载了运算符->,去访问Array原始类型对象的成员
        p1->show();
        
        //智能指针也重载了运算符*,获得原始类型Array的对象
        //意思就是取*表示这个类的一个对象
        (*p1).show();
        
        //获得原始类型的指针
        Array *pArray = p1.get();
        pArray->show();
        
        
        //第二个共享指针
        //use_count来获得有多少个共享智能指针在使用这块内存，引用计数器嘛
        cout << "reference count = " << p1.use_count();
        shared_ptr<Array> p2 = p1;
        
        //reset表示智能指针不再拥有这个内存的使用权，同时引用计数器会减一
        p2.reset();
    }


    main()
    {
		func();//输出构造函数和析构函数
          
            return 0;
    }
    
-----------------------------------------------------------------------------    
    unique：独占指针，同一时刻只能有一个
    
        
    #include <memory>    
    class Array
    {
        public:
        	Array(int len):len(len),p(nullptr)
            {
                cout << "Array构造" << endl;
                if(len > 0)
                {
                    p = new int[len];
                    for(int i = 0;i < len;i++)
                    {
                        p[i] = i+100;
                    }
                }
            }
        
        	~Array()
            {
                cout << "Array析构" << endl;
                if(p != nullptr)
                {
                    delete []p;
                    p = nullptr;
                }
            }
        
        	void show()
            {
                for(int i = 0;i < len;i++)
                {
                    cout << p[i] << ' ';
                }
                cout << endl;
            }
        private:
        	int len;
        	int *p;
    };
    
    void func()
    {
        //方式一
        unique_ptr<Array> p1(new Array (100));
        
        //方式二,make_unique是一个模板函数，对独占资源的智能指针分配空间
        unique_ptr<Array> p1 = make_unique<Array>(100);
        
        //重载->
        p1->show();
        
        //重载*
        (*p1).show();
        
        //获得原始类型的指针
        Array *pA = p1.get();
        pA->show();
       
        //ERR,既然是独占的，那么就不能赋值给另一个同类型的指针
        unique_ptr<Array> p2 = p1;
        unique_ptr<array> p2 = make_unique<Array>(100);
        p2 = p1;//ERR,也不能调用赋值运算符
        
        //将p1独占资源的智能指针指向的内存的所有权转移给p2
        unique_ptr<Array> p2 = move(p1);//转移构造
        if(p1 == nullptr)
        {
            cout << "p1 is a nullptr" << endl;
        }
        else
        {
            cout << "p1 sill active" << endl;
        }
        //p1将会变成nullptr空指针
        
        return 0;
        
    }
    
--------------------------------------------------------------------------------
    weak:弱指针
   	#include <memory>
        class B;
        class A
        {
            public:
            	A()
                {
                    cout << "A构造" << endl;
                }
            	~A()
                {
                    cout << "A析构" << endl;
                }
            	void show()
                {
                    cout << "A show()" << endl;
                }
            	//shared_ptr<B> p;
            	//共享资源的智能指针赋值给弱智能指针，不会引起引用计数器的增加
            	//弱指针其实是共享资源的智能指针的观察者
            	weak_ptr<B> p;
        };
    
    
    	 class B
        {
            public:
            	B()
                {
                    cout << "B构造" << endl;
                }
            	~B()
                {
                    cout << "B析构" << endl;
                }
            	void show()
                {
                    cout << "B show()" << endl;
                }
            	//shared_ptr<A> p;
             	weak_ptr<A> p;
        };
    
		void func()
        {
            //make_shared会为共享指针分配内存空间
            shared_ptr<A> pA = make_shared<A>();
            shared_ptr<B> pB = make_shared<B>();
            
            pA->p = pB;
            Pb->p = pA;
            
            pA->show();
            pB-<show();
        }

		main()
        {
            func();
            
            
            return 0;
        }



~~~

STL

```C++
STL：标准模板库
    #include <vector>:向量
    

	void printf(vector<int>& v)
    {
    	//动态数组遍历第二种方式
        for(int item: v)
        {
            cout << item << ' ';
        }
    	cout >> endl;
    }
	
    main()
	{
    	vector<int> intVect;
        for(int i = 0;i < 10;i++)
        {
            intVect.push_back(i+1);//往尾部插入数据
        }
        for(int i = 0;i < 10;i++)
        {
            //动态数组遍历的第一种方式
            cout << intVect[i] << ' ';
        }
        cout << endl;
        //
        cout << "size = " << intVect.size() << endl;
        //数组能容纳最大元素个数，能够根据插入的个数动态的扩充容量
        cout << "capacity = " << intVect.capacity() << endl;
        
        printf(intVect);
        
        //第三种方式
        for(int i = 0;i < intVect.size();i++)
        {
            cout << intVect.at(i) << ' ';
        }
        cout << endl;
 
        //第四种：使用正向迭代器遍历
        vector<int>::iterator it;
        for(it = intVect.begin();it != intVect.end();it++)
        {
            //迭代器是一个指针，获得相应元素的值
            cout << (*it) << ' ';
        }
        cout << endl;
        
        //使用反向迭代器
        for(auto it = intVect.rbegin();it != intVect.rend();it++)
        {
            cout << (*it) << ' ';
        }
        cout << endl;
        
        
        
        
        
        //删除
         for(int i = 0;i < 10;i++)
         {
             //从尾部删除数据
             intVect.pop_back();
         }
        
        //删除的第二种方式
        //从头部删除100个数据
        intVect.erase(intVect.begin(),intVect.begin()+100);
        
        //删除的第三种方式
        //删除所有的数据
        intVect.clear();
        
        //释放多余的不用的内存
        intVect.shrink_to_fit();
        //能够将capacity减小到当前已有数据大小
        
        return 0;
	}
```

关联式容器

```C++
map:
	#include <map>


	//第一种赋值
	void print(int i,map<string,string>& m)
    {
        for(auto item: m)
        {
            cout << item.first << '-->' << item.second << "|";
        }
        cout << endl;
    }

	main()
    {
        map<string,string> id_name_map = {
            {"0912001","张三"},
            {"0912002","李四"},
            {"0912003","王五"}
        };
 		print(1,id_name_map);       
        
        //通过重载[]运算符来插入数据
        id_name_map["0912004"] = "李二狗";
        print(1,id_name_map);
        
        //修改已经存在的数据
        id_name_map["0912001"] = "李大狗";
        print(1,id_name_map);
        
        //通过insert方式插入数据
        id_name_map.insert(pair<string,string>("0912005","离散狗"));
        print(1,id_name_map);
        
        //使用find方法查找数据
        auto it = id_name_map.find("0912004");
        if(it != id_name_map.end())
        {
            cout << "the data is " << endl;
            cout << it->first << "-- >" << it->second << endl;
        }
        else
        {
            cout << "no find" << endl;
        }
        
        
        return 0;
    }
```

