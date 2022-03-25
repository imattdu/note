+++
title = "循环"
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

### while



```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
   int i = 10;
   while (i != 0) {
       cout << i;
       i--;
   }
}

```





输入

EOF:结束符

1

```cpp
// 读不到就会返回flase
while (cin >> x)
```

2

```cpp
// cin >> x, x 
// 逗号表达式 以最后一个表达式为准 
while(cin >> x, x)
```

3

```cpp
 while(scanf("%d", &x) != -1)
```

4

```cpp
while(~scanf("%d", &x))
```



输出%

```cpp
printf("%%");
```





### do while



```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
    int i = 0;
    do {
        cout << i;
        i++;
    } while (i != 0);
}

```





### for



```cpp
#include <iostream>

using namespace std;

int main()
{
    int i = 0;
    for (i; i < 10; i++) cout << i;
}
```



### break continue

break跳出当前循环

continue:跳出本次循环

















```cpp
#include <iostream>

using namespace std;

int main() {
    int n, a = 1, b = 1;
    cin >> n;
    int i = 0;
    while (i < n - 1) {
        int c = a + b;
        a = b;
        b = c;
        i++;
    }
    
    cout << a;
    return 0;
}
```





循环快

递归慢







曼哈顿距离



```cpp
|x - x1| + |y - y1|
```







```cpp
#include <iostream>

using namespace std;

int main() {
    int n;
    cin >> n;
    int mid = n / 2;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if ((abs(i - mid) + abs(j - mid)) <= mid) {
                cout << "*";
            } else {
                cout << " ";
            }
        }
        cout << endl;
    }
    
    return 0;
}
```



### 连续奇数的和 

给定两个整数 XX 和 YY，输出在他们之间（不包括 XX 和 YY）的所有奇数的和。

#### 输入格式

第一行输入 XX，第二行输入 YY。

#### 输出格式

输出一个整数，表示所有满足条件的奇数的和。

#### 数据范围

−100≤X,Y≤100−100≤X,Y≤100

#### 输入样例1：

```
6
-5
```

#### 输出样例1：

```
5
```

#### 输入样例2：

```
15
12
```

#### 输出样例2：

```
13
```

#### 输入样例3：

```
12
12
```

#### 输出样例3：

```
0
```





注意正负数

```cpp
#include <iostream>
#include <cmath>

using namespace std;

int main() {
    int X, Y, t, res;
    cin >> X >> Y;
    if (X > Y) {
        t = Y;
        Y = X;
        X = t;
    }
    res = 0;
    for (int i = X + 1; i < Y; i++) {
        if (abs(i % 2) == 1) res += i;
    }
    cout << res;

    return 0;
}
```



声明多变量



```cpp
#include <iostream>

using namespace std;

int main() {
    int n = 1, index = 0;
    cout << n << index;
    return 0;
}
```







algorithm



swap(x, y)







### 连续奇数的和



输入 NN 对整数对 X,YX,Y，对于每对 X,YX,Y，请你求出它们之间（不包括 XX 和 YY）的所有奇数的和。

#### 输入格式

第一行输入整数 NN，表示共有 NN 对测试数据。

接下来 NN 行，每行输入一组整数 XX 和 YY。

#### 输出格式

每对 X,YX,Y 输出一个占一行的奇数和。

#### 数据范围

1≤N≤1001≤N≤100,
−1000≤X,Y≤1000−1000≤X,Y≤1000

#### 输入样例：

```
7
4 5
13 10
6 4
3 3
3 5
3 4
3 8
```

#### 输出样例：

```
0
11
5
0
0
0
12
```







1  -1 数据范围

```cpp
#include <iostream>

using namespace std;

int main() {
    long int n, x, y;
    cin >> n;
    
    for (int i = 0; i < n; i++) {
        cin >> x >> y;
        if (x > y) swap(x, y);
        int res = 0;
        for (int j = x + 1; j < y; j++) {
            if (j % 2 == 1 || j % 2 == -1) res += j;
        }
        cout << res << endl;
        
    }
    return 0;
}
```







不赋值 它的值 不确定







超时





CPP:1s 一亿次





完全数



一个整数，除了本身以外的其他所有约数的和如果等于该数，那么我们就称这个整数为完全数。

例如，66 就是一个完全数，因为它的除了本身以外的其他约数的和为 1+2+3=61+2+3=6。

现在，给定你 NN 个整数，请你依次判断这些数是否是完全数。

#### 输入格式

第一行包含整数 NN，表示共有 NN 个测试用例。

接下来 NN 行，每行包含一个需要你进行判断的整数 XX。

#### 输出格式

每个测试用例输出一个结果，每个结果占一行。

如果测试数据是完全数，则输出 `X is perfect`，其中 XX 是测试数据。

如果测试数据不是完全数，则输出 `X is not perfect`，其中 XX 是测试数据。

#### 数据范围

   1≤X≤10^8

#### 输入样例：

```
3
6
5
28
```

#### 输出样例：

```
6 is perfect
5 is not perfect
28 is perfect
```

```cpp
 
using namespace std;

int main() {
    long int N, x;
    cin >> N;
    while (N--) {
        cin >> x;
        long int sum = 0;
        for (long int i = 1; i * i <= x ; i++) {
            if (x % i == 0) {
                if (i < x) sum += i;
                if (i != x / i && x / i < x) sum += x / i;
            }
        }
        if (sum == x) cout << x << " is perfect\n";
        else cout << x << " is not perfect\n";
    }
    
    return 0;
}
```

成对出现的





6 

2 

========



3