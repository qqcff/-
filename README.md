# -2.23
-------  
把它初始化为nullptr或者0，这样程序就能检测并知道它有没有指向一个具体的对象了，在此前提下，判断p是否指向合法的对象，只需把p作为if的条件即可，如果怕的值是nullptr，则条件为假，反之，条件为真。

2.24
----------  
p是合法的，因为void*是一种特殊的指针类型，可用于存放任意对象的地址。
lp是合法的，因为lp是一个长整型指针，而i只是一个普通型整数，二者的类型不匹配。

2.25
-------  
(a)ip是一个整型指针，指向一个整型数，它的值是所指整型数在内存中的地址；i是一个整型数；r是一个引用，它绑定了i，r的值就是i的值。
(b)i是一个整型数；ip是一个整型指针，它的值被初始化为0.
(c)ip是一个整型指针，指向一个整型数它的值是所指整型数在内存中的地址；ip2是一个整型数。

2.35  
---------
j是整数；k是整型常量；p是指向整型常量的指针；j2，k2是整数。  
程序：


	#inlclude<iostream>
	#include<typeinfo>
	
	int main()
	{       const int i=42;
	        auto j=i;
		const auto &k=i;
		auto *p=&i;
		const auto j2=i,&k2=i;
		std::cout << typeid(i).name() << std::endl;
		std::cout << typeid(j).name() << std::endl;
		std::cout << typeid(k).name() << std::endl;
		std::cout << typeid(p).name() << std::endl;
		std::cout << typeid(j2).name() << std::endl;
		std::cout << typeid(k2).name() << std::endl;
		return 0;
	}

3.4  
-------
比较字符串大小


	#include<iostream>
	#include<string>
	
	using namespace std;
	
	int main()
	{             string s1,s2;
	              cout << "请输入两个字符串: " << endl;
	              cin >> s1 >> s2;
	              if(s1 == s2)
	                     cout << "两个字符串相等" << endl;
	              else if (s1 > s2)
	                     cout << s1 << "大于 " << s2 << endl;
	              else
	                     cout << s2 << "大于 " << s1 << endl;
	              return 0;
	}
字符串长度比较 
 
	#include <iostream>
	#include <string>
	using namespace std;
	void main()
	{	
		string mystring1 , mystring2;
		cin>>mystring1>>mystring2;
		if (mystring1.size() != mystring2.size())
		{
			cout<<(mystring1.size() >= mystring2.size() ? mystring1 : mystring2)<<endl;
		}
		else
		{
			cout<<"The length of these strings are the same!"<<endl;
		}	
	}

3.5
------
连接字符串  

	#include <iostream>
	#include <string>
	using namespace std;
	void main()
	{	
		string mystring;
		string sumstring;
		while (getline(cin ,mystring))
		{
			sumstring += mystring;
			cout<<sumstring<<endl;
		}
	}	

分隔开来  

	#include <iostream>
	#include <string>
	using namespace std;
	void main()
	{	
		string mystring;
		string sumstring;
		while (getline(cin ,mystring))
		{
			sumstring = sumstring+mystring+" ";
			cout<<sumstring<<endl;
		}
	}

3.20
----------  
求相邻元素和

	#include <iostream>
	#include <string>
	#include <vector>
	using namespace std;
	void main()
	{	
		vector<int> My_vector;
		int a[10];
		for (int i = 0;i<10;i++)
		{
			cin>>a[i];
		}
		for (int j = 0;j<10;j++)
		{
			My_vector.push_back(a[j]);
		}
		int sum[10];
		for (int k = 0;k<10;k++)
		{
			sum[k] = My_vector[k] + My_vector[k+1];
			cout<<sum[k]<<endl;
			k++;
		}
	} 
  求首尾元素和


	#include <iostream>
	#include <string>
	#include <vector>
	using namespace std;
	void main()
	{	
		vector<int> My_vector;
		int a[10];
		for (int i = 0;i<10;i++)
		{
			cin>>a[i];
		}
		for (int j = 0;j<10;j++)
		{
			My_vector.push_back(a[j]);
		}
		int sum[10];
		for (int k = 0;k<5;k++)
		{
			sum[k] = My_vector[k] + My_vector[9-k];
			cout<<sum[k]<<endl;
		}
	} 

3.23
----------  

	#include <iostream>
	#include <string>
	#include <vector>
	using namespace std;
	void main()
	{	
		vector<int> text(10,5);
		for (auto it = text.begin(); it != text.end();it++) //注意判断其是否为空
		{
			*it = *it * 2;
			cout<<*it<<endl;	
		}
	} 

6.10
--------

	#include<iostream>
	using namespace std;
	void mySWAP(int *p,int*q)
	{       int *tmp=p;
		p=q;
		q=tmp;
	}
	int main()
	{
		int a=5,b=10;
		int *r=&a,*s=&b;
		cout<<"交换前: a="<<",b="<<b<<endl;
		mySWAP(r,s);
		cout<<"交换后: a="<<",b="<<b<<endl;
		return 0;
	}

6.19
-----------  
(a)：函数只有一个参数，传入两个不合法.  
(b)：合法  
(c)：合法  
(d)：合法  

6.39 
----- 
(a)：非法，顶层const不影响传入函数的对象  
(b)：非法，函数的重载必须有形参数量或者形参类型上的不同  
(c)：两个函数是重载关系，它们的形参类型有区别  

7.16
-----  
需要控制的类的相关操作—类成员的初始化、拷贝、赋值、销毁对象  
private隐藏类的相关实现细节，实现封装。  
访问说明符的作用域是开始知道下一个访问说明符或者类结束。不想被使用该类的程序看到的代码细节，都要private.

7.27 
----- 

	class Screen
	{
	private:
		unsigned height=0,width=0;
		usigned cursor=0;
		string contents;
	public:
		Screen()=default;
		Screen(unsigned ht,unsigned wd) :height(ht),width(wd),contents(ht*wd,' ') { }
		Screen(unsigned ht,unsigned wd,char c)
		   : height(ht),width(wd),contents(ht*wd,c){ }
	public:
		Screen&move(unsigned r,unsigned c)
		{cursor =r*width+c;
		return this;
		}
		Screen&set(char ch)
		{contents[cursor]=ch;
		return *this;
		}
		Screen&set(unsigned r,unsigned c,char ch)
		{contents[r*width+c]=ch;
		return *this;
		}
		Screen&display()
		{cout<<contents;
		return *this;
	      }
	};

7.49  
-------
(a)合法   
(b)不合法，Salesdata&类型与Salesdata类型之间不可转换       
(c)不合法，const不对，因为combine本身是需要改变传入参数的    

7.58 
----- 
在类的内部，rate和wec的初始化是错误的，因为除了静态常量成员之外，其他静态成员不能在类的内部初始化。另外，example.c文件的两条语句也是错误的，因为在这里我们必须给出静态成员的初始值。
