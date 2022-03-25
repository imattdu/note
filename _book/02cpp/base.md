+++
title = "cpp基础"
date = 2022-01-20T17:25:20+08:00
featured = false
comment = true
toc = true
reward = true
weight = 9
categories = [
  "tool"
]
tags = [
]
series = [
]
images = []
aliases = [
]

+++

file使用

<!--more-->







## 基础知识



### demo



```cpp
#include <iostream>

using namespace std;

int main() {
    // endl 回车
    cout << "hello word" << endl;
    return 0;
}
```



### 变量



#### 变量的定义



在函数内部定义的变量没有初始化其值不确定

函数外会有默认值

```cpp
#include <iostream>

using namespace std;

int main() {
    int a = 10;
    int b = 2, c, d = 1;
    // 错误
    // int b, c = 1, 2;
    cout << a << b << c << d;
    return 0;
} 
```

#### 变量类型



![](https://raw.githubusercontent.com/imattdu/img/main/img/202110101255038.png)



![](https://raw.githubusercontent.com/imattdu/img/main/img/202110101300996.png)



-2147483648 -2147483647



1 Byte = 8 bit  





long long int : 32位 使用 long long int 64 long int



```
float i = 1.1f;
double j = 1.2;
long double k = 1.1;
```



%d %lld

long long int i = 1ll;

%f %lf %llf





### 输入输出

**cstdio 比 iostream快一点**

#### iostream

```cpp
#include <iostream>

using namespace std;

int main() {
    int i = 1;
    cin >> i;
    cout << i << endl;
    return 0;
}
```



#### cstdio



```cpp
#include <cstdio>

using namespace std;

int main() {
    long long int i;
    scanf("%lld", &i);
    printf("%lld", i);
    return 0;
}

```



- int：%d
- gloat: %f, 默认保留6位小数
- double: %lf， 默认保留6位小数
-    

- float, double等输出保留若干位小数时用：%.4f, %3lf
- %8.3f, 表示这个浮点数的最小宽度为8，保留3位小数，当宽度不足时在前面补空格。
- %-8.3f，表示最小宽度为8，保留3位小数，当宽度不足时在后面补上空格
- %08.3f, 表示最小宽度为8，保留3位小数，当宽度不足时在前面补上0

### 算数运算





![](https://raw.githubusercontent.com/imattdu/img/main/img/202110250205428.png)



#### **俩个计算自动转为精度高的数据类型**



x % y

结果和x正负一直







#### 前加加 后加加 前减减 后减减



#### 类型转换

```cpp
long long int i = 10ll;
int j = (int)i;
```

















pow（double, double） 几次方

sqrt(int) 开 

  

































100 50 20 10 5 1



尽量使用大的，数量用的就少



string





```c++
#include <cstdio>
#include <iostream>
#include <string>

using namespace std;

int main() {
    string name;
    double base;
    double ex;
    
    cin >> name;
    scanf("%lf %lf", &base, &ex);
    double res = base + 0.15 * ex;
    printf("TOTAL = R$ %.2lf", res);
    return 0;
}
```







abs(-1)  1

abs(-1.5) 1.5



## 题目





读取一个带有两个小数位的浮点数，这代表货币价值。

在此之后，将该值分解为多种钞票与硬币的和，每种面值的钞票和硬币使用数量不限，要求使用的钞票和硬币的数量尽可能少。

钞票的面值是 100,50,20,10,5,2100,50,20,10,5,2。

硬币的面值是 1,0.50,0.25,0.10,0.051,0.50,0.25,0.10,0.05 和 0.010.01。

#### 输入格式

输入一个浮点数 NN。

#### 输出格式

参照输出样例，输出每种面值的钞票和硬币的需求数量。

#### 数据范围

0≤N≤1000000.000≤N≤1000000.00

#### 输入样例：

```
576.73
```

#### 输出样例：

```
NOTAS:
5 nota(s) de R$ 100.00
1 nota(s) de R$ 50.00
1 nota(s) de R$ 20.00
0 nota(s) de R$ 10.00
1 nota(s) de R$ 5.00
0 nota(s) de R$ 2.00
MOEDAS:
1 moeda(s) de R$ 1.00
1 moeda(s) de R$ 0.50
0 moeda(s) de R$ 0.25
2 moeda(s) de R$ 0.10
0 moeda(s) de R$ 0.05
3 moeda(s) de R$ 0.01
```



```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main() {
    
    double N;
    cin >> N;
    int n = (int)(N * 100);
    
    printf("NOTAS:\n");
    printf("%d nota(s) de R$ 100.00\n", n / 10000);n %= 10000;
    printf("%d nota(s) de R$ 50.00\n", n / 5000);n %= 5000;
    printf("%d nota(s) de R$ 20.00\n", n / 2000);n %= 2000;
    printf("%d nota(s) de R$ 10.00\n", n / 1000);n %= 1000;
    printf("%d nota(s) de R$ 5.00\n", n / 500);n %= 500;
    printf("%d nota(s) de R$ 2.00\n", n / 200);n %= 200;
    printf("MOEDAS:\n");
    printf("%d moeda(s) de R$ 1.00\n", n / 100);n %= 100;
    printf("%d moeda(s) de R$ 0.50\n", n / 50);n %= 50;
    printf("%d moeda(s) de R$ 0.25\n", n / 25);n %= 25;
    printf("%d moeda(s) de R$ 0.10\n", n / 10);n %= 10;
    printf("%d moeda(s) de R$ 0.05\n", n / 5);n %= 5;
    printf("%d moeda(s) de R$ 0.01\n", n / 1);
    
    return 0;
}
```











