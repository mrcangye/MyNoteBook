## 从 C 到 C++ 语言的转变

#### 1.平时使用还以 printf, scanf 为主

printf 和 scanf 与 cin cout 相比效率更快

#### 2.头文件改成如下，C++对C语言兼容

```c++
#include <cmath> // 相当于C语⾔⾥⾯的#include <math.h>
#include <cstdio> // 相当于C语⾔⾥⾯的#include <stdio.h>
#include <cctype> // 相当于C语⾔⾥⾯的#include <ctype.h>
#include <cstring> // 相当于C语⾔⾥⾯的#include <string.h>
```

#### 3.算法中推荐使用const变量而不是define

```c++
const int i=123456789;
```

4.string类比char[]处理字符串更方便，但是string只能用cin cout输入输出

```C++
string s = "hello world"; // 赋值字符串
string s2 = s;
string s3 = s + s2; // 字符串拼接直接⽤+号就可以
string s4;
len = s.length();
cin >> s4; // 读⼊字符串
cout << s; // 输出字符串
```

cin的读入以空格为分隔符，如果读入一整行就需要getline(cin,s)

substr可以用来截取子串

```
string s2 = s.substr(4); // 表示从下标4开始⼀直到结束
string s3 = s.substr(5, 3); // 表示从下标5开始，3个字符
```

#### 5.C++书写结构体不需要写关键字struct

```C++
struct stu {
 int grade;
 float score;
};
struct stu arr1[10]; // C语⾔⾥⾯需要写struct法
stu arr2[10];// C++⾥⾯不⽤写
```

#### 6.C++中的引用

```C++
void func(int &a) { // 传⼊的是n的引⽤，相当于直接对n进⾏了操作，只不过在func函数
中换了个名字叫a
 a = 99;
}
int main() {
 int n = 0;
 func(n); // n由0变成了99
}
```

#### 7.vector的使用

```C++
#include <iostream>
#include <vector>
using namespace std;
int main() {
 	vector<int> a; // 定义的时候不指定vector的⼤⼩
 	cout << a.size() << endl; // 这个时候size是0
    for (int i = 0; i < 10; i++) {
	a.push_back(i); // 在vector a的末尾添加⼀个元素i
 }
 cout << a.size() << endl; // 此时会发现a的size变成了10
 vector<int> b(15); // 定义的时候指定vector的⼤⼩，默认b⾥⾯元素都是0
 cout << b.size() << endl;
 for (int i = 0; i < b.size(); i++) {
 b[i] = 15;
 }
```

