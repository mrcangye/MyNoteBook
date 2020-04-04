# 1002 A+B for Polynomials (25分)

This time, you are supposed to find *A*+*B* where *A* and *B* are two polynomials.

### Input Specification:

Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:

*K* N~1~ ~aN1~ *N*~2aN2~ ... N~KaNK~

where *K* is the number of nonzero terms in the polynomial, N~i~ and ~aNi~ (*i*=1,2,⋯,*K*) are the exponents and coefficients, respectively. It is given that 1≤*K*≤10，0≤N~K~<⋯<N~2~<N~1~≤1000.

### Output Specification:

For each test case you should output the sum of *A* and *B* in one line, with the same format as the input. Notice that there must be NO extra space at the end of each line. Please be accurate to 1 decimal place.

### Sample Input:

```in
2 1 2.4 0 3.2
2 2 1.5 1 0.5
```

### Sample Output:

```out
3 2 1.5 1 2.9 0 3.2
```



这一题的题意是完成两个多项式的相加。

例如示例的：

2.4x + 3.2 与 1.5x^2^ +0.5x 相加结果为 1.5x^2^+2.9x+3.2



这题的主要思想是将ｘ是几次方作为数组的下标，具体的值作为数组的值。例如x^2^就可以表示为s[2] = 1。

我们初始化的这个数组，要让其值全为０。每次输入一个就将其与对应的值相加。这样就可以得到最后的数值结果了。

为了减少循环的次数，我们可以标记一个最大的ｘ次方为max，它就是这个数组最长的下标。后面循环的时候，只需要以这个最长的下标处作为参照，不用一直到数组的末尾。

```
#include<cstdio>
#include<iostream>
using namespace std;
int main()
{
    int k;
    int m,n,sum=0,max = 0;
    double t;
    double a[1001]={0};
    for(int j = 0;j<2;j++) {
        cin >> m;
        for (int i = 0; i < m; i++) {
            scanf("%d %lf", &k, &t);
            a[k] += t;
            if(k>max)
                max =k;
        }
    }
    for(int j=0;j<=max;j++)
    {
        if(a[j]!=0)
            sum++;
    }
    cout << sum;
    for(int j=max;j>=0;j--)
    {
        if(a[j]!=0)
            printf(" %d %.1f",j,a[j]);
    }
    return 0;
}
```