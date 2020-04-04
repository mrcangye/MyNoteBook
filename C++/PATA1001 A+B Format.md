# PATA1001 A+B Format

[PATA1001 A+B Format](https://pintia.cn/problem-sets/994805342720868352/problems/994805528788582400)

> Calculate a+b and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (unless there are less than four digits)



Sample Input:

`-1000000 9`

Sample Output:

`-999,991`



主要的工作是对输出的结果进行处理，怎么样使得逗号正确的分隔是关键。

我们可以使用 ` to string ` 将 ` A+B ` 的数字结果转化为字符串。

按照题意去用逗号分隔字符串。

`to_string` 的用法：[to_string](http://www.cplusplus.com/reference/string/to_string/)， **推荐查阅官方文档，表述清晰**

```cpp
#include <iostream>
using namespace std;
int main() {
    int a, b;
    cin >> a >> b;
    string s = to_string(a + b); //将输入的a+b的结果转化为字符串
    //接下来我们处理字符串就可以了
    int len = s.length();//计算字符串长度
    for (int i = 0; i < len; i++) {
        cout << s[i];
        if (s[i] == '-')
            continue;
        if ((i + 1) % 3 == len % 3 && i != len - 1)
            //判断是否需要加逗号
            cout << ",";
    } 
    return 0;
}c
```



```cpp
if ((i + 1) % 3 == len % 3 && i != len - 1)        
            cout << ",";
```

上面这段可能不好理解，接下来我试着解释清楚

我们用 `len % 3` 将这个字符串里的字符分为 `3` 个一组。这个余数的数值代表着三个一组多出来了几个。我们从**右边往左边** 3 个一组开始分。分到最左边，要么刚刚好，要么多了1个或者两个。

1. 如果刚刚好，那么第一个逗号的位置应该是，字符串从左往右数，**第** `**3**` 个数值后面，也就是字符串**下标**为 `2` 的位置，后面的逗号位置就是前一个逗号位置往后推 `3` 个。
2. 如果余 `1` ，那么第一个逗号的位置应该是，字符串从左往右数，**第** `**1**` 个数值后面，也就是字符串**下标**为 `0` 的位置，后面的逗号位置就是前一个逗号位置往后推 `3` 个。
3. 如果余 `2` ，那么第一个逗号的位置应该是，字符串从左往右数，**第** `**2**` 个数值后面，也就是字符串**下标**为 `1` 的位置，后面的逗号位置就是前一个逗号位置往后推 `3` 个。

这样判断条件 `(i + 1) % 3 == len % 3 ` 就出来了。

然而，字符串末尾肯定是不能加逗号的。

所以我们要加上一句 ` i != len - 1` 