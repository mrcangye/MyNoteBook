## 从 C 到 C++ 语言的转变

#### 1.平时使用还以 printf, scanf 为主

printf 和 scanf 与 cin cout 相比效率更快

#### 2.头文件改成如下，C++对C语言兼容

```cpp
#include <cmath> // 相当于C语⾔⾥⾯的#include <math.h>
#include <cstdio> // 相当于C语⾔⾥⾯的#include <stdio.h>
#include <cctype> // 相当于C语⾔⾥⾯的#include <ctype.h>
#include <cstring> // 相当于C语⾔⾥⾯的#include <string.h>
```

#### 3.算法中推荐使用const变量而不是define

```cpp
const int i=123456789;
```

#### 4.string类比char[]处理字符串更方便，但是string只能用cin cout输入输出

```cpp
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

```cpp
string s2 = s.substr(4); // 表示从下标4开始⼀直到结束
string s3 = s.substr(5, 3); // 表示从下标5开始，3个字符
```

#### 5.C++书写结构体不需要写关键字struct

```cpp
struct stu {
 int grade;
 float score;
};
struct stu arr1[10]; // C语⾔⾥⾯需要写struct法
stu arr2[10];// C++⾥⾯不⽤写
```

#### 6.C++中的引用

```cpp
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

```cpp
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
 vector<int> c(20, 2); // 定义的时候指定vector的⼤⼩并把所有的元素赋⼀个指定
的值
 for (int i = 0; i < c.size(); i++) {
 cout << c[i] << " ";
 }
 cout << endl;
 for (auto it = c.begin(); it != c.end(); it++) { // 使⽤迭代器的⽅式访
问vector
 cout << *it << " ";
 } 
 return 0;
}
```

#### 8.set集合

set的集合内容是各不相同的，set内的元素按小到大排序

```cpp
#include <iostream>
#include <set>
using namespace std;
int main() {
 set<int> s; // 定义⼀个空集合s
 s.insert(1); // 向集合s⾥⾯插⼊⼀个1
 cout << *(s.begin()) << endl; // 输出集合s的第⼀个元素 (前⾯的星号表示要对
指针取值)
 for (int i = 0; i < 6; i++) {
 s.insert(i); // 向集合s⾥⾯插⼊i
 }
 for (auto it = s.begin(); it != s.end(); it++) { // ⽤迭代器遍历集合s
⾥⾯的每⼀个元素
 cout << *it << " ";
 }
 cout << endl << (s.find(2) != s.end()) << endl; // 查找集合s中的值，如果结果等于s.end()表示未找到 (因为s.end()表示s的最后⼀个元素的下⼀个元素所在的位置)
 cout << (s.find(10) != s.end()) << endl; // s.find(10) != s.end()表
示能找到10这个元素
    s.erase(1); // 删除集合s中的1这个元素
 cout << (s.find(1) != s.end()) << endl; // 这时候元素1就应该找不到啦～
 return 0;
}
```

#### 9.map

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main() {
 map<string, int> m; // 定义⼀个空的map m，键是string类型的，值是int类型的
 m["hello"] = 2; // 将key为"hello", value为2的键值对(key-value)存⼊map中
 cout << m["hello"] << endl; // 访问map中key为"hello"的value, 如果key不
存在，则返回0
 cout << m["world"] << endl;
 m["world"] = 3; // 将"world"键对应的值修改为3
 m[","] = 1; // 设⽴⼀组键值对，键为"," 值为1
 // ⽤迭代器遍历，输出map中所有的元素，键⽤it->first获取，值⽤it->second获取
 for (auto it = m.begin(); it != m.end(); it++) {
 cout << it->first << " " << it->second << endl;
 }
 // 访问map的第⼀个元素，输出它的键和值
 cout << m.begin()->first << " " << m.begin()->second << endl;
 // 访问map的最后⼀个元素，输出它的键和值
 cout << m.rbegin()->first << " " << m.rbegin()->second << endl;
 // 输出map的元素个数
 cout << m.size() << endl;
 return 0;
}
```



#### 10.stack

```cpp
#include <iostream>
#include <stack>
using namespace std;
int main() {
 stack<int> s; // 定义⼀个空栈s
 for (int i = 0; i < 6; i++) {
 s.push(i); // 将元素i压⼊栈s中
 }
 cout << s.top() << endl; // 访问s的栈顶元素
 cout << s.size() << endl; // 输出s的元素个数
 s.pop(); // 移除栈顶元素
 return 0;
}
```

#### 11.queue

```cpp
#include <iostream>
#include <queue>
using namespace std;
int main() {
 queue<int> q; // 定义⼀个空队列q
 for (int i = 0; i < 6; i++) {
 q.push(i); // 将i的值依次压⼊队列q中
 }
 cout << q.front() << " " << q.back() << endl; // 访问队列的队⾸元素和队
尾元素
 cout << q.size() << endl; // 输出队列的元素个数
 q.pop(); // 移除队列的队⾸元素
 return 0;
}
```

#### 12.unordered_map和unordered_set

`unordered_map`在头⽂件` #include <unordered_map> `中，`unordered_set`在头⽂件 `#include
<unordered_set> `中～
unordered_map和map（或者unordered_set和set）的区别是，map会按照键值对的键key进⾏排序
（set⾥⾯会按照集合中的元素⼤⼩进⾏排序，从⼩到⼤顺序），⽽unordered_map（或者
unordered_set）省去了这个排序的过程，如果偶尔刷题时候⽤map或者set超时了，可以考虑⽤
unordered_map（或者unordered_set）缩短代码运⾏时间、提⾼代码效率～⾄于⽤法和map、set
是⼀样的

#### 13.bitset

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
bool cmp(int a, int b) { // cmp函数返回的值是bool类型
 return a > b; // 从⼤到⼩排列
}
int main() {
 vector<int> v(10);
 for (int i = 0; i < 10; i++) {
 cin >> v[i];
 }
 sort(v.begin(), v.end());// 因为这⾥没有传⼊参数cmp，所以按照默认，v从⼩到
⼤排列
 
 int arr[10];
 for (int i = 0; i < 10; i++) {
 cin >> arr[i];
 }
 sort(arr, arr + 10, cmp); // arr从⼤到⼩排列，因为cmp函数排序规则设置了从
⼤到⼩
 return 0;
}
```

#### 14.cmp

```cpp
#include <iostream>
using namespace std;
struct stu { // 定义⼀个结构体stu，number表示学号，score表示分数
 int number;
 int score;
}
bool cmp(stu a, stu b) { // cmp函数，返回值是bool，传⼊的参数类型应该是结构体
stu类型
 if (a.score != b.score) // 如果学⽣分数不同，就按照分数从⼤到⼩排列
 return a.score > b.score;
 else // 如果学⽣分数相同，就按照学号从⼩到⼤排列
 return a.number < b.number;
}
// 有时候这种简单的if-else语句我喜欢直接⽤⼀个C语⾔⾥⾯的三⽬运算符表示～
bool cmp(stu a, stu b) {
 return a.score != b.score ? a.score > b.score : a.number <
b.number;
}
```

sort默认是从⼩到⼤排列的，也可以指定第三个参数cmp函数，然后⾃⼰定义⼀个cmp函数指定排序
规则～cmp最好⽤的还是在结构体中，尤其是很多排序的题⽬～⽐如⼀个学⽣结构体stu有学号和成绩
两个变量，要求如果成绩不同就按照成绩从⼤到⼩排列，如果成绩相同就按照学号从⼩到⼤排列，那
么就可以写⼀个cmp数组实现这个看上去有点复杂的排序过程

#### 15.cctype

```cpp
#include <iostream>
#include <cctype>
using namespace std;
int main() {
 char c;
 cin >> c;
 if (isalpha(c)) {
 cout << "c is alpha";
 }
 return 0;
}
```

-   [**isalnum**](http://www.cplusplus.com/reference/cctype/isalnum/)检查是否是字母数字

-   [**isalpha**](http://www.cplusplus.com/reference/cctype/isalpha/)检查是否是字母

-   [**isblank** ](http://www.cplusplus.com/reference/cctype/isblank/)检查是否是空白

-   [**iscntrl**](http://www.cplusplus.com/reference/cctype/iscntrl/)控制字符

-   [**isdigit**](http://www.cplusplus.com/reference/cctype/isdigit/)十进制数字

-   [**islower**](http://www.cplusplus.com/reference/cctype/islower/)小写字母 tolower是小写

-   [**isprint**](http://www.cplusplus.com/reference/cctype/isprint/)是否可打印

-   [**ispunct**](http://www.cplusplus.com/reference/cctype/ispunct/)标点符号

-   [**isspace**](http://www.cplusplus.com/reference/cctype/isspace/)空格

-   [**isupper**](http://www.cplusplus.com/reference/cctype/isupper/)大写字母 toupper转为大写

-   [**isxdigit**](http://www.cplusplus.com/reference/cctype/isxdigit/)十六进制数字



#### 16.C++11 auto

auto是C++11⾥⾯的新特性，可以让编译器根据初始值类型直接推断变量的类型

如果想要在Dev-Cpp⾥⾯使⽤C++11特性的函数，⽐如刷算法中常⽤的stoi、to_string、
unordered_map、unordered_set、auto这些，需要在设置⾥⾯让dev⽀持c++11～需要这样做～
在⼯具-编译选项-编译器-编译时加⼊这个命令“-std=c++11”即可

在STL中使⽤迭代器的时候，auto可以代替⼀⼤⻓
串的迭代器类型声明：

```cpp
// 本来set的迭代器遍历要这样写：
for(set<int>::iterator it = s.begin(); it != s.end(); it++) {
 cout << *it << " ";
}
// 现在可以直接替换成这样的写法：
for(auto it = s.begin(); it != s.end(); it++) {
 cout << *it << " ";
}
```

#### 17. c_str()

将 string 格式的串转化为 C 语言中的数组

例如，输入：

```cpp
// strings and c-strings
#include <iostream>
#include <cstring>
#include <string>

int main ()
{
  std::string str ("Please split this sentence into tokens");

  char * cstr = new char [str.length()+1];
  std::strcpy (cstr, str.c_str());

  // cstr now contains a c-string copy of str

  char * p = std::strtok (cstr," ");
  while (p!=0)
  {
    std::cout << p << '\n';
    p = std::strtok(NULL," ");
  }

  delete[] cstr;
  return 0;
}
```

```out
//输出为：
Please
split
this
sentence
into
tokens
```

