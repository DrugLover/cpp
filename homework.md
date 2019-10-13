[TOC]

#### 第二章

###### 2.23 

给定指针p，你能知道它是否指向了一个合法的对象吗?如果能，叙述判断的思路；如果不能，也请说明原因。

​	答：不能，无法知道指针p是否被初始化

###### 2.24

在下面这段代码中为什么p合法而lp非法？

```c++
int i = 42; 
void *p = &i;
long *lp = &i;
```

​	答：void *p可以指向任意对象，而long *lp指向int型，所以非法

###### 2.25

说明下列变量的类型和值。

```c++
(a)  int* ip,i,&r = i;
(b)  int i,*ip=0;
(c)  int* ip,ip2;
```

​	答：(a)ip是int型指针，i是int型变量，r是int型引用

​			(b)i是int型变量，ip是空指针

​			(c)ip是int型指针，ip2是int型变量

###### 2.35

判断下列定义 推断出的类型是什么，然后编写程序进行验证。

```c++
const int i = 42;
auto j = i;
const auto &k = i;
auto *p = &i;
const auto j2 = i, &k2 = i;
```

​	答：j是const int型；k是const int &型；p是const int *型；j2是const int型；k2是const int &型

#### 第三章

###### 3.4

编写一段程序读入两个字符串，比较其是否相等并输出结果。如果不相等，输出较大的那个字符串。改写上述程序，比较输入的两个字符串是否等长，如果不等长，输出长度较大的那个字符串。

```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
int main() {
    std::string a, b;
    std::cin >> a >> b;
    if (a != b) {
        if (a > b) {
            std:: cout << a << '\n';
        } else {
            std:: cout << b << '\n';
        }
    } else {
        std:: cout << "Two strings are equal";
    }
    return 0;
}
```



```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
int main() {
    std::string s1, s2;
    std::cin >> s1 >> s2;
    int a = s1.length();
    int b = s2.length();
    if (a != b) {
        if (a > b) {
            std:: cout << s1 << '\n';
        } else {
            std:: cout << s2 << '\n';
        }
    } else {
        std:: cout << "Two strings\' length are equal";
    }
    return 0;
}
```

###### 3.20

读入一组整数并把它们存入一个vector对象，将每对相邻整数的和输出出来。改写你的程序，这次要求先输出第1个和最后1个元素的和，接着输出第2个和倒数第2个元素的和，以此类推。

```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
int main() {
    int x, n;
    std:: vector<int> a;
    std:: cout << "Please input the number of integers\n";
    std:: cin >> n;
    for (int i = 0; i < n; i++) {
        std:: cin >> x;
        a.push_back(x);
    }
    for (int i = 0; i < n-1; i++) {
        std:: cout << a[i] + a[i+1] << ' ';
    }
    return 0;
}
```



```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
int main() {
    int x, n;
    std:: vector<int> a;
    std:: cout << "Please input the number of integers\n";
    std:: cin >> n;
    for (int i = 0; i < n; i++) {
        std:: cin >> x;
        a.push_back(x);
    }
    for (int i = 0; i < n/2; i++) {
        std:: cout << a[i] + a[n-i-1] << ' ';
    }
    if (n%2 == 1) {
        std:: cout << a[n/2];
    }
    return 0;
}
```

###### 3.23

编写一个程序，创建一个含有10个整数的vector对象，然后使用迭代器将所有元素的值都变成原来的两倍。输出vector对象的内容，检验程序是否正确。

```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
int main() {
    int x, n;
    std:: vector<int> a(10);
    for (int i = 1; i <= 10; i++) {
        a[i-1] = i;
    }
    for (int i = 0; i < 10; i++) {
        std:: cout << a[i] << ' ';
    }
    std:: cout << '\n';
    for (auto it = a.begin(); it != a.end(); it++) {
        *it = *it *  2;
        std:: cout << *it << ' ';
    }
    return 0;
}
```

#### 第六章

###### 6.10

编写一个函数，使用指针形参交换两个整数的值。在代码中调用该函数并输出交换后的结果，以此验证函数的正确性。

```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
void change(int *a, int *b) {
    int t = *a;
    *a = *b;
    *b = t;
}
int main() {
    int a = 1, b = 2;
    int *p = &a, *q = &b;
    change(p, q);
    std:: cout << a << b;
    return 0;
}
```



###### 6.19

假定有如下声明，判断哪个调用合法，哪个调用不合法。对于不合法的函数调用，说明原因。

```c++
double calc(double);
int count(const string &, char);
int sum(vector<int>::iterator, vector<int>::iterator, int);
vector<int> vec(10);
```

(a) calc(23.4, 55.1)								(b) count("abcda", 'a');

(c) calc(66)											 (d) sum(vec.begin(), vec.end(), 3.8);

答：(a)不合法，函数只有一个参数，传入两个。(b),(c),(d)合法

###### 6.39

说明在下面的每组声明中第二条声明语句是何含义。如果有非法的声明，请指出来。

(a) int calc(int, int);

​	  int calc(const int, const int)

(b) int get();

​	  double get();

(c) int *reset(int *);

​	  double *reset(double *);

答：(a).   第二句非法，和第一句重复声明

​		(b).   第二句非法，和第一句形参一样，返回类型不一样

​		(c).   第二句合法

#### 第七章

###### 7.16

在类的定义中对于访问说明符出现的位置和次数有限定吗？如果有，是什么？什么样的成员应该定义在public说明符之后？什么样的成员应该定义在private说明符之后？ 

答：无限制。构造函数和作为接口的成员函数应定义public之后。数据成员和作为实现的函数应定义在private之后。

###### 7.27



给你自己的screen类添加move、set和display函数，通过执行下面的代码检验你的类是否正确。

```c++
Screen myScreen(5, 5, ' X');
myScreen.move(4,0).set(' #').display(cout);
cout << "\n";
myScreen.display(cout);
cout << "\n";
```

答：

```c++
// Copyright 2019 Songhao
#include<bits/stdc++.h>
class Screen {
 public:
    typedef std::string::size_type pos;
    Screen() = default;
    Screen(pos ht, pos wd, char c) : height(ht), width(wd), contents(ht * wd, c) {}
    char get() const {
        return contents[cursor];
    }
    inline char get(pos ht, pos wd) const;
    Screen &move(pos r, pos c);
    Screen &set(char);
    Screen &set(pos, pos, char);
    Screen &display(std::ostream &os) {
        do_display(os);
        return *this;
    }
    const Screen &display(std::ostream &os) const {
        do_display(os);
        return *this;
    }

 private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
    void do_display(std::ostream &os) const {
        os << contents;
    }
};

inline Screen& Screen::move(pos r, pos c) {
    pos row = r * width;
    cursor = row + c;
    return *this;
}

char Screen::get(pos r, pos c) const {
    pos row = r * width;
    return contents[row + c];
}

inline Screen &Screen::set(char c) {
    contents[cursor] = c;
    return *this;
}

inline Screen &Screen::set(pos r, pos col, char ch) {
    contents[r * width + col] = ch;
    return *this;
}

int main() {
    Screen myScreen(5, 5, ' X');
    myScreen.move(4, 0).set(' #').display(std:: cout);
    std:: cout << "\n";
    myScreen.display(std:: cout);
    std:: cout << "\n";
}
```

###### 7.49

对于combine函数的三种不同声明，当我们调用i.combine(s)时分别发生声明情况？其中i是一个Sales_data, 而s是一个string对象

```c++
(a) Sales_data &combine(Sales_data);
(b) Sales_data &combine(Sales_data&);
(c) Sales_data &combine(const Sales_data&) const;
```

答：a正确，编译器给s创建一个Sales_data对象并传给combine的形参，函数执行并返回结果

​		b错误，combine的参数是非常量，而s是string，无法传参

​		c错误，combine声明成了常量成员函数，无法修改数据成员的值



###### 7.58

下列静态数据成员的声明和定义有错误吗？请解释原因。

```c++
//example.h
class Example {
 public:	
	static double rate = 6.5;
	static const int vecSize = 20;	
	static vector<double> vec(vecSzie);
};
//example.c
#include "example.h"
double Example::rate;
vector<double> Example::vec;
```

答：

example.h中rate和vecSize的初始化是错误的，因为他们不是静态常量成员，其他静态成员不能在类的内部初始化。

example.c中定义也是错的，未给出初始值。



