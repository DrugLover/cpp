[TOC]



### 第九章

###### 9.11

对6种创建和初始化vector对象的方法，每一种都给出一个实例。解释每个vector包含什么值。



``` c++
vector<int> vec;			//0
vector<int> vec(10);		//0
vector<int> vec(10,1);		//1
vector<int> vec(1,2,3);		//1,2,3
vector<int> vec(vec1);		//与vec1一样
vector<int> vec(vec1.begin(),vec1.end());//与vec1一样
```

###### 9.20

编写程序，从一个list<int>拷贝元素到两个deque中。值为偶数的所有元素都拷贝到哦一个deque中，而奇数值元素都拷贝到另一个deque中。

``` c++
// Copyright Songhao 2019
#include<bits/stdc++.h>
int main() {
    std::list<int> list1;
    for (int i = 0; i < 10; i++) {
        list1.push_back(i);
    }
    std::deque<int> deque1;
    std::deque<int> deque2;
    std::list<int>::iterator it1 = list1.begin();
    for (it1; it1 != list1.end(); it1++) {
        if ((*it1)% 2 == 0) {
            deque1.push_back(*it1);
        } else {
            deque2.push_back(*it1);
        }
    }
    std::deque<int>::iterator it2 = deque1.begin();
    std::deque<int>::iterator it3 = deque2.begin();
    std::cout <<"偶数为";
    for (it2; it2 != deque1.end(); it2++) {
        std::cout <<*it2 <<" ";
    }
    std::cout <<'\n';
    std::cout <<"奇数为";
    for (it3; it3 != deque2.end(); it3++) {
        std::cout <<*it3 <<" ";
    }
    return 0;
}
```

输出结果：

偶数为0 2 4 6 8
奇数为1 3 5 7 9



###### 9.29

假定vec包含25个元素，那么vec.resize(100)会做什么？如果接下来调用vec.resize(10)会做什么？

vec.resize(100)后会把vec扩到100，原来25个元素之后到第100个全部初始化为0

vec.resize(10)后把vec第10个元素之后的全部舍去。



###### 9.43

编写一个函数，接受三个string参数s、oldVal和newVal。使用迭代器及insert和erase函数将s中所有oldVal替换为newVal。测试你的程序，用它替换通用的简写形式，如，将“tho”替换为“though”,将“thru”替换为“through”。

```c++
// Copyright Songhao 2019
#include<bits/stdc++.h>
void fun(std::string &s, std::string oldVal, std::string newVal) {
    std::string::size_type pos = 0;
    while (pos < s.length()) {
        pos = s.find(oldVal, pos);
        if (pos == std::string::npos)
        break;
        s.erase(pos, oldVal.length());
        s.insert(pos, newVal);
        pos = pos + newVal.length();
    }
}
int main() {
    std::string s = "a tho tho thru";
    fun(s, "tho", "though");
    fun(s, "thru", "through");
    std::cout << s << std::endl;
    return 0;
}
```

输出结果：

a though though through

###### 9.52

使用 stack处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从stack中pop对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）push到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所代替。

```c++
#include<bits/stdc++.h>
using namespace std;
int priority(string opt){
	int p = 0;
	if(opt == "(")
		p = 1;
	if(opt == "+" || opt == "-")
		p = 2;
	if(opt == "*" || opt == "/")
		p = 3;
	return p;
}
void calculate(stack<int> &opdstack, string opt){
	if(opt == "+"){
		int ropd = opdstack.top();
		opdstack.pop();
		int lopd = opdstack.top();
		opdstack.pop();
		int result = lopd + ropd;
		opdstack.push(result);
	}
	if(opt == "-")
	{
		int ropd = opdstack.top();
		opdstack.pop();
		int lopd = opdstack.top();
		opdstack.pop();
		int result = lopd - ropd;
		opdstack.push(result);
	}
	if(opt == "*"){
		int ropd = opdstack.top();
		opdstack.pop();
		int lopd = opdstack.top();
		opdstack.pop();
		int result = lopd * ropd;
		opdstack.push(result);
	}
	if(opt == "/"){
		int ropd = opdstack.top();
		opdstack.pop();
		int lopd = opdstack.top();
		opdstack.pop();
		int result = lopd / ropd;
		opdstack.push(result);
	}
}
int calcuExpression(vector<string> vec) {
	stack<int> stack_opd;
	stack<string> stack_opt;
	for (auto i = 0; i != vec.size(); ++i) {
		string token = vec[i];
		if (token == "+" || token == "-" || token == "*" || token == "/"){
			if (stack_opt.size() == 0)
				stack_opt.push(token);
			else {
			 	int token_p = priority(token);
			 	string top_opt = stack_opt.top();
			 	int opt_p = priority(top_opt);
			 	if(token_p > opt_p)
			 		stack_opt.push(token);
			 	else {
			 	  	while(token_p <= opt_p) {
			 	  		stack_opt.pop();
			 	  		calculate(stack_opd, top_opt);
			 	  		if(stack_opt.size() > 0){
			 	  			top_opt = stack_opt.top();
			 	  			opt_p = priority(top_opt);
						} else
						 	break;
					}
					stack_opt.push(token);
				}
			}
		}
		else if(token == "(")
			stack_opt.push(token);
		else if(token == ")") {
			while(stack_opt.top() != "(") {
				string top_opt = stack_opt.top();
				calculate(stack_opd,top_opt);
				stack_opt.pop();
			}
			stack_opt.pop();
		}
		else
		  stack_opd.push(atoi(token.c_str()));  
	}
	while(stack_opt.size() != 0) {
		string top_opt = stack_opt.top();
		calculate(stack_opd,top_opt);
		stack_opt.pop();
	}
	return stack_opd.top();
}
int main() {
	vector<string> tokens =  {"(","1","+","2",")","*","3","/","(","2","-","1",")"};
    for(auto i = 0; i != tokens.size(); ++i)
    cout << tokens[i] << " ";
    cout << "= ";
    cout << calcuExpression(tokens) << endl;
	return 0;
}
```

输出结果：

( 1 + 2 ) * 3 / ( 2 - 1 ) = 9

### 第十章

###### 10.3

用accumulate求一个vector<int>中的元素之和

```c++
// Copyright Songhao 2019
#include<bits/stdc++.h>
int main() {
	int a[10] = {0,1,2,3,4,5};
	std::vector<int> vec(a,a+6);
	std::cout << accumulate(vec.begin(),vec.end(),0);
	return 0;
}
```

输出结果：

15



###### 10.15

编写一个lambda，捕获它所在函数的int，并接受一个int参数。lambda应该返回捕获的int和int参数的值。

```c++
[a](int &b){cout<<a+b;}
```



###### 10.34

使用reverse_iterator逆序打印一个vector

```c++
// Copyright Songhao 2019
#include<bits/stdc++.h>
int main() {
	int a[10] = {0,1,2,3,4,5};
	std::vector<int> vec(a,a+6);
	for (auto it1 = vec.rbegin(); it1 !=vec.rend(); ++it1) {
		std::cout << *it1;
	}
	return 0;
}
```

输出结果：

543210

###### 10.42

使用list代替vector重新实现10.2.3中的去除重复单词的程序。

```c++
// Copyright Songhao 2019
#include<bits/stdc++.h>
int main() {
	std::string a[10] = {"sdc","sddc","sdec","sfdc","sdec","sdc","sdc","fsdc","sadc","fsdc"};
	std::list<std::string> list1(a,a+10);
	list1.sort();
	list1.unique();
	for (auto it1 = list1.begin(); it1 !=list1.end(); ++it1) {
		std::cout << *it1 << " ";
	}
	return 0;
}
```

输出结果：

fsdc sadc sdc sddc sdec sfdc



### 第十一章

###### 11.12

编写程序，读入string和int的序列，将每个string和int存入一个pair中，pair保存在一个vector中。

```c++
//Copyright Songhao 2019
#include<bits/stdc++.h>
int main() { 
	std::vector<std::pair<std::string,int>> vec1(12);
	std::ifstream in1("1.txt");
	std::string str;
	size_t i = 0;
	while (in1>>str) {
		vec1[i].first = str;
		++i;
	}
	std::ifstream in2("2.txt");
	int val;
	size_t j = 0;
	while (in2 >> val) {
		vec1[j].second = val;
		++j;
	}
 
	std::vector<std::pair<std::string,int>>::iterator it1 = vec1.begin();
	std::cout<<"vector中元素为：" << std::endl;
	for (it1; it1 != vec1.end(); ++it1) {
		std::cout << it1->first << " " << it1->second << std::endl;
	}
	return 0;  
}
```

###### 11.17

假定c是一个string的multiset，v是一个string的vector，解释下面的调用。指出每个调用是否合法：

```c++
copy(v.begin(),v.end(),inserter(c,c.end()));
copy(v.begin(),v.end(),back_inserter(c));
copy(v.begin(),c.end(),inserter(v,v.end()));
copy(v.begin(),c.end(),back_inserter(v));
```

1正确 ；2错误，multiset没有push_back操作；3，4正确

###### 11.38

用unordered_map重写单词计数程序和单词转换程序

```c++
#include<bits/stdc++.h>
using namespace std;
int main(){
    unordered_map<string, size_t> word_count; //string到size_t的空map 
    string word;
    while(cin >> word)
        ++word_count[word];        //提取word的计数器并将其加1 
    for(const auto &w : word_count)
    	cout << w.first << " occurs " << w.second << ((w.second > 1) ? " times" : " time") << endl;
    return 0;
} 
```



```c++
#include<bits/stdc++.h>
void wordTransformation(){
	using std::string;
    std::ifstream ifs_map("../data/word_transformation.txt"),
        ifs_content("../data/given_to_transform.txt");
    if (!ifs_map || !ifs_content) {
        std::cerr << "can't find the documents." << std::endl;
        return;
    }
    std::unordered_map<string, string> trans_map;
    for (string key, value; ifs_map >> key && getline(ifs_map, value);)
        if (value.size() > 1)
            trans_map[key] =
                value.substr(1).substr(0, value.find_last_not_of(' '));
    for (string text, word; getline(ifs_content, text); std::cout << std::endl)
        for (std::istringstream iss(text); iss >> word;) {
            auto map_it = trans_map.find(word);
            std::cout << (map_it == trans_map.cend() ? word : map_it->second)
                      << " ";
        }
}

```

### 第十三章

###### 13.12

在下面的代码片段中会发生几次析构函数调用？

```c++
bool fcn(const Sales_data *trans, Sale_data accum) {
    Sales_data item1(*trans), item2(accum);
    return item1.isbn() !=item2.isbn();
}
```

3次，accum,item1,item2



###### 13.18

定义一个Employee类，它包括雇员的姓名和唯一的雇员证号。为这个类定义默认构造函数，以及接受一个表示雇员姓名的string的构造函数。每个构造函数应该通过递增 一个static数据成员来生成一个唯一的证号。

```C++
#ifndef EMPLOYEE_H_INCLUDED
#define EMPLOYEE_H_INCLUDED
 
class Employee{
private:
    static int Seq;
public:
    //默认构造函数
    Employee(): EmployeeNo( Seq++ ) { }
    //接受一个string的构造函数
    Employee( const string &s ): Name(s), EmployeeNo(Seq++) { }
    //拷贝构造函数
    Employee( const Employee &ep ) { Name = ep.Name; EmployeeNo = Seq++; }
    //拷贝赋值运算符
    Employee& operator=( const Employee &ep ) { Name = ep.Name; return *this; }
private:
    string Name;
    int EmployeeNo;
};
 
int Employee::Seq = 0;
 
#endif // EMPLOYEE_H_INCLUDED
```

###### 11.46

什么类型的引用可以绑定到下面的初始化器上？

```c++
int f();
vector<int> v1(100);
int? r1 = f();
int? r2 = vi[0];
int? r3 = r1;
int? r4 = vi[0] * f();
```

(a)：f()为函数的返回值，临时值，属于右值，&&

(b)：vi[0]为变量，属于左值，&

(c)：r1为变量，属于左值，&

(d)：右侧为表达式，属于右值，&&

###### 13.49

为你的StrVec、String和Message类添加一个移动构造函数和一个移动赋值运算符。

```c++
String& String::operator=(String&& rhs) NOEXCEPT
{
	if (this != &rhs) {
		free();
		elements = rhs.elements;
		end = rhs.end;
		rhs.elements = rhs.end = nullptr;
	}
	return *this;
}
String::String(String&& s) NOEXCEPT : elements(s.elements), end(s.end)
{
	s.elements = s.end = nullptr;
}
```

###### 13.58

编写新版本的Foo类，其sorted函数中有打印语句，测试这个类，来验证你对前两题的答案是否正确。

```c++
#include<bits/stdc++.h>
using std::vector;
using std::sort;
class Foo {
public:
	Foo sorted()&&;
	Foo sorted() const&;
 
private:
	vector<int> data;
};
 
Foo Foo::sorted() && {
	sort(data.begin(), data.end());
	std::cout << "&&" << std::endl; // debug
	return *this;
}
 
Foo Foo::sorted() const & {
	std::cout << "const &" << std::endl; 
	return Foo(*this).sorted(); 
}
int main() {
	Foo().sorted(); 
	Foo f;
	f.sorted(); 
}
```

### 第十四章

###### 14.3

string 和vector都定义了重载的==以比较各自的对象，假设svecl和是是sevc2是存放string和vector，确定在下面的表达式中分别使用了哪个版本的==？

(a)"cobble" == "stone"             (b)svec1[0] == svec2[0]

(c)svec1 == svec2                     (d)svec1[0] == "stone"

(a)应用了C++内置版本的==  (b) 应用了string版本的==

(c)应用了vector版本的==      (d)应用了string版本的==

###### 14.25

上题的这个类还需要定义其他赋值运算符吗？如果是，请实现它们；同时说明运算对象应该是说明类型并解释原因。

> 还应该定义赋值运算符，参数为string，将date作为一个字符串赋值给Date类。

```c++
class Date
{
public:
    Date& operator=(const string &date);
    //其他成员
};
Date& Sales_data::operator=(const string &date)
{
    istringstream in(date);
    char ch1, cha2;
    in >> year >> ch1 >> month >> ch2 >> day;
    if (!in || ch1 != '-' || ch2 != '-')
        throw std::invalid_argument("Bad date");
    if (month < 1 || month >12 || day < 1 || day > 31)
        throw std::invalid_argument("Bad date");
    return *this;
}
```

###### 14.38

编写一个类令其检查某个给定的string对象的长度是否与一个阈值相等。使用该对象编写程序，统计并报告在输入的文件中长度为1的单词有多少个、长度为2的单词有多少个、......、长度为10的单词有多少个。

```c++
class IsInRange
{
public:
    IsInRange (std::size_t n) : sz (n) {}
    bool operator() (const std::string& s)
    {
        return s.size() == sz;
    }
    size_t getSize() { return sz; }
private:
 
    std::size_t sz;
};
void test1428()
{
    vector<IsInRange> predicates;
    map<size_t, size_t> len_cnt_table;
 
    for (size_t i = 1; i < 11; ++i) {
        len_cnt_table[i] = 0;
        predicates.push_back(IsInRange(i));
    }
 
    ifstream fi ("C:/Users/tutu/Documents/code/cpp_primer/ch14/storyDataFile.txt");
    string word;
    while (fi >> word) {
        for (auto i : predicates) {
            if (i(word)) {
                ++ len_cnt_table[i.getSize()];
            }
        }
    }
 
    for (auto i : len_cnt_table) {
        cout << i.first << " " << i.second << endl;
    }
}
```



###### 14.52

在下面的加法表达式中分别选用了哪个operator+？列出候选函数、可行函数及为每个可行函数的实参执行的类型转换:

```c++
struct LongDouble {
    LongDouble operator+ (const SmallInt&); // 1
};
LongDouble operator+(LongDouble&, double);  // 2
SmallInt si;
LongDouble ld;
ld = si + ld;
ld = ld + si;
```

ld = si + ld;具有二义性，调用1需将si转换为LongDouble，ld转换为SmallInt。

调用2需要将si转换为LongDouble，ld转换为double。

ld = ld + si;精确匹配LongDouble operator+ (const SmallInt&);

若调用LongDouble operator+(LongDouble&, double);还需将si转换为double。

### 第十五章

###### 15.12

有必要将一个成员函数同时声明称override和final吗?为什么？

有必要；override表示对其基类函数的覆盖，final表示此函数不能再被其他函数覆盖



