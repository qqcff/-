# 9.11
----------  

```cpp
	vector<int> vec;    // 0

	vector<int> vec(10);    // 0

	vector<int> vec(10,1);  // 1

	vector<int> vec{1,2,3,4,5}; // 1,2,3,4,5

	vector<int> vec(other_vec); 

	vector<int> vec(other_vec.begin(), other_vec.end());  
```

9.20
-------  


	#include<iostream>

	#include<fstream>

	#include<sstream>

	#include<string>

	#include<vector>

	#include<list>

	#include<deque>

	using namespace std;



	int main(int argc, char**argv)

	{

		list<int> list1(5,7);

		deque<int> deque1;

		deque<int> deque2;

		list<int>::iterator it1 = list1.begin();

		for (it1; it1 != list1.end(); it1++)

		{

			if ((*it1)%2 == 0)

			{

				deque1.push_back(*it1);

			} 

			else

			{

				deque2.push_back(*it1);

			}

		}



		deque<int>::iterator it2 = deque1.begin();

		deque<int>::iterator it3 = deque2.begin();

		cout<<"偶数为：";

		for (it2; it2 != deque1.end(); it2++)

		{

			cout<<*it2<<" ";

		}

		cout<<endl;

		cout<<"奇数为：";

		for (it3; it3 != deque2.end(); it3++)

		{

			cout<<*it3<<" ";

		}



		return 0;

	}  

9.29 
---------
resize()改变容器的大小，多删少补并初始化，需要默认初始化的，则需要参数类型有默认构造函数。    
resize(100)会将其大小改为100个元素的大小，添加新元素并初始化，之后使用resize(10)会将之后的90个元素舍弃。  

9.43
-------  



	
	#include <iostream>

	#include <string>

	#include <vector>

	using std::cin;

	using std::cout;

	using std::cerr;

	using std::endl;

	using std::vector;

	using std::string;



	void rePlace(string &s, const string &oldVal, const string &newVal)

	{

		auto l = oldVal.size();

		if (!l)

			return;

		auto iter = s.begin();

		while (iter <= s.end()-1) {

			auto iter1 = iter;

			auto iter2 = oldVal.begin();

			while(iter != oldVal.end() && *iter1 == *iter2) {

				++iter1;

				++iter2;

			}

			if(iter2 == oldVal.end()) {

				iter = s.erase(iter, iter1);

				if (newVal.size()) {

					iter2 = newVal.end();

					do {

						--iter2;

						iter = s.insert(iter, *iter2);

					} while(iter2 > newVal.begin());

				}

				iter += newVal.size();

			}

			else ++iter;

		}

	}



	int main()

	{

		string s = "tho thru tho!";

		rePlace(s, "thru", "through");

		cout<<s<<endl;



		rePlace(s, "tho", "though");

		cout<<s<<endl;



		rePlace(s, "through", "");

		cout<<s<<endl;



		return 0;

	}


9.52
----------  
	
	
	
	#include <stack>
	#include <string>
	#include <iostream>

	using std::string; using std::cout; using std::endl; using std::stack;

	int main()
	{
	    string expression{ "This is (pezy)." };
	    bool bSeen = false;
	    stack<char> stk;
	    for (const auto &s : expression)
	    {
		if (s == '(') { bSeen = true; continue; }
		else if (s == ')') bSeen = false;

		if (bSeen) stk.push(s);
	    }

	    string repstr;
	    while (!stk.empty())
	    {
		repstr += stk.top();
		stk.pop();
	    }

	    expression.replace(expression.find("(")+1, repstr.size(), repstr);

	    cout << expression << endl;

	    return 0;
	}


10.3  
-------  


	#include<iostream>

	#include<string>

	#include<vector>

	#include<algorithm>

	#include<numeric>

	using namespace std;



	int main(int argc, char**argv)

	{

		int a[10] = {0,1,2,5,4,5,4,5,4,5};

		vector<int> vec(a,a+10);

		cout<<"元素之和为："<<accumulate(vec.begin(),vec.end(),0);



		return 0;

	}  


10.15 
---------


	


	#include <iostream>



	using namespace std;



	void add(int a)

	{

		//可调用对象

		auto sum = [a](int b) { return a + b; };



		cout << sum(1) << endl;

	}



	int main()

	{

		add(1);

		add(2);



		system("pause");

		return 0;

	}


10.34 
-------



	
	int main() 

	{

		vector<int> v = {1,2,4,5};

		vector<int>::reverse_iterator iter = v.rbegin(), rend = v.rend();

		while(iter != rend)

			cout << *iter++ << ' ';

	    return 0;

	}  


10.42
------


	#include<iostream>  

	#include<fstream>

	#include<string>  

	#include<vector> 

	#include<list>

	#include<algorithm>  

	#include<numeric>  

	#include<functional>

	#include<iterator>

	using namespace std;

	using namespace placeholders;//占位符的命名空间



	int main(int argc, char**argv)  

	{ 

		string a[10] = {"sdc","sddc","sdec","sfdc","sdec","sdc","sdc","fsdc","sadc","fsdc"};

		list<string> list1(a,a+10);

		list1.sort();//使用其成员函数版本的算法，排序

		list1.unique();//删除相同元素

		for (auto it1 = list1.begin(); it1 != list1.end(); ++it1)

		{

			cout<<*it1<<" ";

		}



		return 0;  

	}  

11.17
----------  
(a)合法，将v插入到c的尾部。  
(b)不合法，set中没有push_back  
(c)合法。  
(d)合法。  

11.38
----------  
单词计数程序：  


	#include <unordered_map>
	#include <string>
	#include <iostream>

	using namespace std;

	int main()
	{
		unordered_map<string, size_t> word_count;
		string word;
		while(cin >> word)
			++word_count[word];

		for(const auto &w : word_count)
			cout << w.first << "," << w.second << endl;

		return 0;
	}
单词转换程序：  


	#include <unordered_map>
	#include <iostream>
	#include <string>
	#include <fstream>
	#include <sstream>

	using namespace std;

	unordered_map<string, string> buildMap(ifstream &map_file)
	{
		unordered_map<string, string> trans_map;
		string key;
		string value;
		while(map_file >> key && getline(map_file, value))
			if(value.size() > 1)
				trans_map[key] = value.substr(1);
			else
				throw runtime_error("no rule for " + key);
		return trans_map;
	}

	const string &transform(const string &s, const unordered_map<string, string> &m)
	{
		auto map_it = m.find(s);
		if(map_it != m.cend())
			return map_it->second;
		else
			return s;
	}

	void word_tranform(ifstream &map_file, ifstream &input)
	{
		auto trans_map = buildMap(map_file);
		// for(const auto p : trans_map)
		// 	cout << p.first << "->" << p.second << endl;
		string text;
		while(getline(input, text))
		{
			istringstream stream(text);
			string word;
			bool firstword = true;
			while(stream >> word)
			{
				if(firstword)
					firstword = false;
				else
					cout << " ";
				cout << transform(word, trans_map);
			}
			cout << endl;
		}
	}

	int main()
	{
		ifstream map_file("word_transformation.txt"), input("word_transformation_bad.txt");
		word_tranform(map_file, input);

		return 0;
	}

13.12
--------
这段代码中会发生三次析构函数调用：  

1、函数结束时，局部变量item1的生命期结束，被销毁，Sales_data的析构函数被调用。  

2、类似的，item2在函数结束时被销毁，Sales_data的析构函数被调用。  

3、函数结束时，参数accum的生命期结束，被销毁，Sales_data的析构函数被调用。  

在函数结束时，trans的生命期也结束了，但它是Sales_data的指针，并不是它指向的Sales_data对象的生命期结束（只有delete指针时，指向的动态对象的生命期才结束），所以不会引起析构函数的调用。  


13.18
-----------  


	#include <iostream>
	#include <string>

	class Employee
	{
	friend void print(const Employee&);
	public:
		Employee() { id = n; ++n; };
		Employee(const std::string &s) { id = n; ++n; name = s; };
	private:
		std::string name;
		int id;
		static int n;
	};

	void print(const Employee &e)
	{
		std::cout << e.name << " " << e.id << std::endl;
	}

	int Employee::n = 0;

	int main()
	{
		Employee a;
		Employee b("bbb");

		print(a);
		print(b);

		return 0;
	}


13.46
----- 
1、r1必须是右值引用，因为f是返回非引用类型的函数，返回值是一个右值。  
2、r2必须是左值引用，因为下标运算符返回的是左值。  
3、r3只能是左值引用，因为rl是一个变量，而变量是左值。  
4、r4只能是右值引用，因为vi[0] * f()是一个算数运算表达式，返回右值。  

13.49
-----  


	String& String::operator=(String&& rhs) NOEXCEPT

	{

		if (this != &rhs) {

			free();

			elements = rhs.elements;

			end = rhs.end;

			rhs.elements = rhs.end = nullptr;//将源对象置为可析构的状态

		}

		return *this;

	}

	String::String(String&& s) NOEXCEPT : elements(s.elements), end(s.end)

	{

		s.elements = s.end = nullptr;//将源对象置为可析构的状态

	}


13.58
----- 


	#include <vector>

	#include <iostream>

	#include <algorithm>



	using std::vector;

	using std::sort;



	class Foo {

	public:

		Foo sorted()&&;

		Foo sorted() const&;



	private:

		vector<int> data;

	};



	Foo Foo::sorted() &&

	{

		sort(data.begin(), data.end());

		std::cout << "&&" << std::endl; // debug

		return *this;

	}



	Foo Foo::sorted() const &

	{

		//    Foo ret(*this);

		//    sort(ret.data.begin(), ret.data.end());

		//    return ret;



		std::cout << "const &" << std::endl; // debug



		//    Foo ret(*this);

		//    ret.sorted();     //13.56

		//    return ret;



		return Foo(*this).sorted(); //13.57

	}



	int main()

	{

		Foo().sorted(); // call "&&"

		Foo f;

		f.sorted(); // call "const &"

	}


14.3 
-------
(a)"cobble" == "stone"应用了C++语言内置版本的==，比较两个指针。  

(b)svec1[0] == svec2[0]应用了string版本的重载==。  

(c)svec1 = svec2应用了vector版本的重载==。  

(d)svec1[0] == "stone"应用了string版本的重载==，字符串字面常量被转换成string  


14.20
----- 



	class Sales_data

	{

		friend Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs);

	public:

		Sales_data& operator+=(const Sales_data &rhs);

		//其他成员

	};

	Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs)

	{

		Sales_data sum = lhs;

		sum += rhs;

		return sum;

	}

	Sales_data& Sales_data::operator+=(const Sales_data &rhs)

	{

		units_sold += rhs.units_sold;

		revenue += rhs.revenue;

		return *this;

	}

14.38
--------




	#include <iostream>

	#include <vector>

	#include <string>

	#include <algorithm>



	using namespace std;



	class StrLenIs

	{

	public:

		StrLenIs(int len) : len(len) { }

		bool operator()(const string &str) { return str.length() == len; }



	private:

		int len;

	};



	void readStr(istream &is, vector<string> &vec)

	{

		string word;

		while (is >> word)

		{

			vec.push_back(word);

		}

	}



	int main()

	{

		vector<string> vec;

		readStr(cin, vec);

		const int minLen = 1;

		const int maxLen = 10;

		for (int i = minLen; i <= maxLen; ++i)

		{

			StrLenIs slenIs(i);

			cout << "len: " << i << ", cnt: " << count_if(vec.begin(), vec.end(), slenIs) << endl;

		}



		system("pause");

		return 0;

	}

14.52
----------
对于ld=si+ld，由于LongDouble不能转换为SmallInt，因此Smallint的成员operator+和friend operator都不可行。  
由于Smallint不能转换为LongDouble，LongDouble的成员operator+和非成员operator+也都不可行。  

由于SmallInt可以转换为int， LongDouble了可以转换为float和double，所以内置的operator+(int, float)和operator+(int, double)都可行，会产生二义性。  

对于ld=ld+si，类似上一个加法表达式，由于Smallint不能转换为double，LongDouble也不能转换为SmallInt，因此SmallInt的成员operator+和两个非成员operator+都不匹配。  

LongDouble的成员operator+可行，且为精确匹配。  
SmallInt可以转换为int，longDouble可以转换为float和double，因此内置的operator+(float, int)和operator(double, int)都可行。但它们都需要类型转换，因此LongDouble的成员operator+优先匹配。  

15.13
--------





	#include <iostream>

	#include <string>



	using namespace std;



	class base

	{

	public:

		base(string szNm) : basename(szNm) { }

		string name() { return basename; }

		virtual void print(ostream &os) { os << basename; }

	private:

		string basename;

	};

	class derived : public base

	{

	public:

		derived(string szName, int iVal) : base(szName), mem(iVal) { }

		void print(ostream &os) { base::print(os); os << " " << mem; }

	private:

		int mem;

	};



	int main()

	{

		base r1("机器学习");

		r1.print(cout);

		derived r2("机器学习", 10);

		r2.print(cout);



		system("pause");

		return 0;

	}

15.16
-------



	class Limited_quote : public Disc_quote

	{

	public:

		Limited_quote() = default;

		Limited_quote(const string &book, double price, size_t qty, double disc) : 

			Disc_quote(book, price, qty, disc) { }

		double net_price(size_t cnt) const override

		{

			if (cnt <= quantity)

			{

				return cnt * (1 - discount) * price;

			}

			else

			{

				return quantity * (1 - discount) * price + (cnt - quantity) * price;

			}

		}



	};
15.30
---------



	class Basket

	{

	public:

		void add_item(const shared_ptr<Quote> &sales)

		{

			items.insert(sales);

		}

		double total_receipt(std::ostream&) const;     // 打印每本书的总价和购物篮中所有书的总价

	private:

		static bool compare(const std::shared_ptr<Quote> &lhs, const std::shared_ptr<Quote> &rhs)

		{

			return lhs->isbn() < rhs->isbn();

		}

		// multiset保存多个报价，按照compare成员排序

		std::multiset<std::shared_ptr<Quote>, decltype(compare)*> items{ compare };

	};



	double Basket::total_receipt(std::ostream &os) const

	{

		double sum = 0.0;



		for (auto iter = items.cbegin(); iter != items.cend(); iter = items.upper_bound(*iter))

		{

			sum += print_total(os, **iter, items.count(*iter));

		}

		os << "Total Sale: " << sum << endl;

		return  sum;

	}
