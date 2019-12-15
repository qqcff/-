# 12.1
----------  

由于StrBlob的data成员是一个指向string的vector的shared_ptr，因此StrBlob的赋值不会拷贝vector的内容，而是多个StrBlob对象共享同一个（创建于动态内存空间上）vector对象。b1和b2共享的是底层数据，因此，b1和b2均包含4个string。  

12.10
-------  

此调用是正确的，利用p创建一个临时的shared_ptr赋予process的参数ptr，p和ptr都指向相同的int对象，引用计数被正确地置为2。process执行完毕后，ptr被销毁，引用计数减1，这是正确的——只有p指向它。  

12.15
---------



	#include <iostream>

	#include <memory>



	using namespace std;



	struct destination {};

	struct connection {};



	connection connect(destination *pd)

	{

		cout << "打开连接" << endl;

		return connection();

	}



	void disconnect(connection c)

	{

		cout << "关闭连接" << endl;

	}



	//未使用shared_ptr的版本

	void f(destination &d)

	{

		cout << "直接管理connect" << endl;

		connection c = connect(&d);

		//忘记调用disconnect关闭连接



		cout << endl;

	}



	//void end_connection(connection *p) { disconnect(*p); }



	//使用shared_ptr的版本

	void f1(destination &d)

	{

		cout << "用shared_ptr管理connect" << endl;

		connection c = connect(&d);



		shared_ptr<connection> p(&c, [](connection *p) { disconnect(*p);  });

		//忘记调用disconnect关闭连接



		cout << endl;

	}



	int main(int argc, char *argv[])

	{

		destination d;

		f(d);



		f1(d);



		system("pause");

		return 0;

	}


12.17
-------  




	int ix = 1024, *pi = &ix, *pi2 = new int(2048);

	typedef unique_ptr<int> IntP;

	(a)IntP p0(ix);

	//不合法。unique_ptr需要用一个指针初始化，无法将int转换为指针。

	(b)IntP p1(pi);

	//合法。可以用一个int*来初始化IntP，但此程序逻辑上是错误的。

	//它用一个普通int变量的地址初始化p1,p1销毁时会释放此内存，其行为是未定义的。

	(c)IntP p2(pi2);

	//合法。用一个指向动态分配的对象的指针来初始化IntP是正确的。

	(d)IntP p3(&ix);

	//合法。但存在与(b)相同的问题。

	(e)IntP p4(new int(2048));

	//合法。与(c)类似。

	(f)IntP p5(p2.get());

	//合法。但用p2管理的对象的地址来初始化p5，造成两个unique_ptr指向相同的内存地址。

	//当其中一个unique_ptr被销毁（或调用reset释放对象）时，

	//该内存被释放，另一个unique_ptr变为空悬指针

 



12.18
----------  
	
	
unique_ptr独占对象的所有权，不能拷贝和赋值。release操作是用来将对象的所有权转移给另一个unique_ptr的。  
而多个shared_ptr可以共享对象的所有权。需要共享时，可以简单拷贝和赋值。因此，并不需要release这样的操作来转移所有权。  

12.19 
-------  

	#ifndef MY_STRBLOB_H

	#define MY_STRBLOB_H

	#include <vector>

	#include <string>

	#include <initializer_list>

	#include <memory>

	#include <stdexcept>



	using namespace std;



	//对于StrBlob的友元声明来说，此前置声明是必要的

	class StrBolbPtr;



	class StrBlob

	{

		//友元类

		friend class StrBlobPtr;

	public:

		typedef vector<string>::size_type size_type;

		StrBlob();

		StrBlob(initializer_list<string> il);

		size_type size() const { return data->size(); }

		bool empty() const { return data->empty(); }

		//添加和删除元素

		void push_back(const string &t) { data->push_back(t); }

		void pop_back();

		//元素访问

		string& front();

		const string& front() const;

		string& back();

		const string& back() const;



		//提供给StrBlobPtr的两个接口

		StrBlobPtr begin();          //定义StrBlobPtr后才能定义这两个函数

		StrBlobPtr end();

	private:

		shared_ptr<vector<string>> data;

		//如果data[i]不合法，抛出一个异常。

		void check(size_type i, const string &msg) const;

	};



	inline StrBlob::StrBlob() : data(make_shared<vector<string>>()) { }

	StrBlob::StrBlob(initializer_list<string> il) : data(make_shared<vector<string>>(il)) { }



	inline void StrBlob::check(size_type i, const string &msg) const

	{

		if (i >= data->size())

		{

			throw out_of_range(msg);

		}

	}



	inline string& StrBlob::front()

	{

		//如果vector为空，check会抛出一个异常

		check(0, "front on empty StrBolb");

		return data->front();

	}



	//const版本的front

	inline const string& StrBlob::front() const

	{

		check(0, "front on empty StrBlob");

		return data->front();

	}



	inline string& StrBlob::back()

	{

		check(0, "back on empty StrBlob");

		return data->back();

	}



	//const版本的back

	inline const string& StrBlob::back() const

	{

		check(0, "back on empty StrBlob");

		return data->back();

	}



	inline void StrBlob::pop_back()

	{

		check(0, "pop_back on empty StrBlob");

		return data->pop_back();

	}



	//当试图访问一个不存在的元素时，StrBlobPtr抛出一个异常

	class StrBlobPtr

	{

		friend bool eq(const StrBlobPtr&, const StrBlobPtr&);

	public:

		StrBlobPtr() :curr(0) { }

		StrBlobPtr(StrBlob &a, size_t sz = 0) :wptr(a.data), curr(sz) { }



		string& deref() const;

		StrBlobPtr& incr();         //前缀递增

		StrBlobPtr& decr();         //前缀递减

	private:

		//若检查成功，check返回一个指向vector的shared_ptr

		shared_ptr<vector<string>> check(size_t, const string&) const;

		//保存一个weak_ptr，意味着底层vector可能会被销毁

		weak_ptr<vector<string>> wptr;

		size_t curr;       //在数组中的当前位置



	};



	inline shared_ptr<vector<string>> StrBlobPtr::check(size_t i, const string &msg) const

	{

		auto ret = wptr.lock();      //vector还存在吗？

		if (!ret)

		{

			throw runtime_error("unbond StrBlobPtr");

		}

		if (i > ret->size())

		{

			throw out_of_range(msg);

		}

		return ret;          //否则，返回指向vector的shared_ptr

	}



	inline string& StrBlobPtr::deref() const

	{

		auto p = check(curr, "dereference past end");

		return (*p)[curr];    //(*p)是对象所指向的vector

	}



	//前缀递增：返回递增后的对象的引用

	inline StrBlobPtr& StrBlobPtr::incr()

	{

		//如果curr已经指向容器的尾后位置，就不能递增它

		check(curr, "increment past end of StrBlobPtr");

		++curr;

		return *this;

	}



	//前缀递减：返回递增后的对象的引用

	inline StrBlobPtr& StrBlobPtr::decr()

	{

		//如果curr已经为0，递减它就会产生一个非法下标

		--curr;           //递减当前位置

		check(-1, "decrement past begin of StrBlobPtr");

		return *this;

	}



	//StrBlob的begin和end成员的定义

	inline StrBlobPtr StrBlob::begin()

	{

		return StrBlobPtr(*this);

	}



	inline StrBlobPtr StrBlob::end()

	{

		auto ret = StrBlobPtr(*this, data->size());

		return ret;

	}



	//StrBlobPtr的比较操作

	inline bool eq(const StrBlobPtr &lhs, const StrBlobPtr &rhs)

	{

		auto l = lhs.wptr.lock(), r = rhs.wptr.lock();

		//若底层的vector是同一个

		if (l == r)

		{

			//则两个指针都是空，或者指向相同元素时，它们相等

			return (!r || lhs.curr == rhs.curr);

		}

		else

		{

			return false;            //若指向不同vector，则不可能相等

		}

	}



	inline bool neq(const StrBlobPtr &lhs, const StrBlobPtr &rhs)

	{

		return !eq(lhs, rhs);

	}



	#endif

12.30
---------


	class QueryResult; //为了定义query的返回类型，这个定义是必须的

	class TextQuery

	{

	public:

		using line_no = vector<string>::size_type;

		TextQuery(ifstream&);       //构造函数

		QueryResult query(const string&) const;



	private:

		shared_ptr<vector<string>> file;  //输入文件

		//每个单词到它所在行号的集合的映射

		map<string, shared_ptr<set<line_no>>> wm;



	};



	//读取输入文件并建立单词到行号的映射

	TextQuery::TextQuery(ifstream& is) : file(new vector<string>)

	{

		string text;

		while (getline(is, text))       //对文件中每一行

		{

			file->push_back(text);      //保存此行文本

			int n = file->size() - 1;   //当前行号

			istringstream line(text);   //将行文本分解为单词

			string word;

			while (line >> word)        //对行中每个单词

			{

				//如果单词不在wm中，以之为下标在wm中添加一项

				auto &lines = wm[word]; //lines是一个shared_ptr

				if (!lines)            //在我们第一次遇到这个单词时，此指针为空

				{

					lines.reset(new set<line_no>);  //分配一个新的set

				}

				lines->insert(n);      //将此行号插入set中

			}

		}

	}

	class QueryResult

	{

		friend ostream& print(ostream&, const QueryResult&);

	public:

		QueryResult(string s, shared_ptr<set<line_no>> p, shared_ptr<vector<string>> f):

			sought(s), lines(p), file(f) { }

	private:

		string sought;     //查询单词

		shared_ptr<set<line_no>> lines;

		shared_ptr<vector<string>> file;

	};



	QueryResult TextQuery::query(const string &sought) const

	{

		//如果未找到sought，我们将返回一个指向次set的指针

		static shared_ptr<set<line_no>> nodata(new set<line_no>);

		//使用find而不是下标运算符来查找单词，避免将单词添加wm中

		auto loc = wm.find(sought);

		if (loc == wm.end())

		{

			return QueryResult(sought, nodata, file);

		}

		else

		{

			return QueryResult(sought, loc->second, file);

		}

	}


16.11
-------



	在List类内，使用模版名不需要再加参数列表，而ListItem使用时必须加上<>模版参数列表  

16.12
------

	template <typename T> class Blob 

	{

	public:

		typedef T value_type;

		typedef typename std::vector<T>::size_type size_type;



		// constructors

		Blob();

		Blob(std::initializer_list<T> il);





		// number of elements in the Blob

		size_type size() const { return data->size(); }

		bool empty() const { return data->empty(); }





		// add and remove elements

		void push_back(const T &t) { data->push_back(t); }





		void push_back(T &&t) { data->push_back(std::move(t)); }

		void pop_back();





		// element access

		T& back();

		T& operator[](size_type i); // defined in § 14.5 (p. 566)





	private:

		std::shared_ptr<std::vector<T>> data;



		// throws msg if data[i] isn't valid

		void check(size_type i, const std::string &msg) const;

	};

	int main()

	{



		Blob<int> ia; // empty Blob<int>

		Blob<int> ia2 = { 0,1,2,3,4 }; // Blob<int> with five elements





		system("pause");

		return 0;

	}

 

16.19
----------  

	#ifndef HAVE_H

	#define HAVE_H



	template <typename T> void Have(T &t)

	{

		for (size_t i = 0; i < t.size(); ++i)

		{

			cout<<t[i]<<endl;

		}

	}

	#endif HAVE_H

	#include <iostream>

	#include <vector>

	#include <list>

	#include <string>

	#include "Have.h"

	using namespace std;



	int main(int argc,char** argv)

	{

		vector<int> vec1;

		vec1.push_back(2);

		vec1.push_back(3);

		Have(vec1);

		system("pause");

		return 0;

	}


16.41
----------  

	#ifndef REU_TYPE_H

	#define REU_TYPE_H



	template <typename T> auto sum(const T&a,const T&b) ->decltype(a+b)//将函数的返回类型指定为a+b的类型

	{

		return a+b;

	}

	#endif REU_TYPE_H

	#include <iostream>

	#include <vector>

	#include <list>

	#include <string>

	#include "Reu_type.h"

	using namespace std;





	int main(int argc,char** argv)

	{

		int a = 566669;

		int b = 595955;

		cout<<sum(a,b);

		cin.get();

		return 0;

	}


16.62
--------

	#ifndef SHOW_TIMES_H

	#define SHOW_TIMES_H



	template<typename T, typename U> int show_times(const T& vec,const U& val)

	{

		int showtimes = 0;

		for (size_t i = 0; i < vec.size(); ++i)

		{

			if(vec[i] == val)

			{

				++showtimes;

			}

		}

		return showtimes;

	}

	#endif SHOW_TIMES_H



	#include <iostream>

	#include <vector>

	#include <list>

	#include <string>

	#include "show_times.h"

	using namespace std;



	int main(int argc,char** argv)

	{

		vector<int> vec1;

		vec1.push_back(1);

		vec1.push_back(2);

		int a = 1;

		cout<<a<<"出现次数为："<<show_times(vec1,a)<<endl;



		vector<double> vec2;

		vec2.push_back(1.2);

		vec2.push_back(2.4);

		double b = 1.2;

		cout<<b<<"出现次数为："<<show_times(vec2,b)<<endl;



		vector<string> vec3;

		vec3.push_back(string("123"));

		vec3.push_back(string("23"));

		string c = "123";

		cout<<c<<"出现次数为："<<show_times(vec3,c)<<endl;

		cin.get();

		return 0;

	}


