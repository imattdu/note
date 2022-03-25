+++
title = "数组"
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

# 数组

## 基本知识



### 一维数组



#### 声明



```cpp
#include <iostream>

using namespace std;

int main() {
    
    int a[5], b[10];
    return 0;
}
```

#### 初始化

a  索引2 后面的没有赋值就是0

```cpp
#include <stdio.h>

int main()
{
    int a[10] = {1, 2, 3};
    return 0;
}
```



### 二维数组



```cpp
#include <iostream>

using namespace std;

int main() {
    int a[3][3] = {
        {1, 2, 3},
        {1, 2, 3},
        {1, 2, 3}
    } ;
    
    
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            a[i][j] = i + j;
        }
    }
    
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << a[i][j] << endl;
        }
    }
    
    
    return 0;
}
```







```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    int a[10];
    // const 常量
    // 40 byte int = 4byte
    memset(a, -1, 40);
    // s返回数组字节大小
    // a 复制给 b
    memcpy(b, a, sizeof a);
    memset(a, 0 ,sizeof a);
    for (int i = 0; i < 10; i++) {
        cout << a[i] << endl;
    }
    
    
    return 0;
}
```







函数外堆

函数内栈



默认栈空间1mb局部变量值随机的



全局变量是0



```cpp
#include <iostream>

using namespace std;

int main() {
    
    int a[3] = {0, 1, 2};
    int b[] = {1, 2, 3};
    // 没有给出的值 默认为0
    int c[5] = {1, 2, 3};
    int d[10] = {0};
    // 不初始化 值是随机的
    
    

    
    
    return 0;
}
```









## 题目

### 数组翻转k次



```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    int a[5] = {1, 2, 3, 4, 5};
    int k;
    cin >> k;
    // [start, end)
    reverse(a, a + 5);
    reverse(a, a + k);
    reverse(a + k, a + 5);
    
    
    
    for (int i = 0; i < 5; i++) {
        cout << a[i];
    }
    return 0;
}
```







fabs()

abs()

sqrt() double





```cpp
#include <iostream>
#include <cstdio>
#include <cmath>

using namespace std;

int main() {
    int i = 3;
    printf("%.20lf", sqrt(i) * sqrt(i));
    
    return 0;
}
```





### 高精度计算



2^n



```cpp
#include <iostream>

using namespace std;

const int N = 3000;

int main() {
    int a[N] = {1};
    int m = 1, n;
    
    cin >> n;
    for (int i = 0; i < n; i++) {
        int t = 0;
        for (int j = 0; j < m; j++) {
            t += a[j] * 2;
            a[j] = t % 10;
            t = t / 10;
        }
        if (t) a[m++] = 1;
    }
    
    for (int i = m - 1; i >= 0; i--) cout << a[i];
    
    
}
```













==========





```cpp
min(1, 2);
max(1, 2);
```













输入整数 NN，输出一个 NN 阶的回字形二维数组。

数组的最外层为 11，次外层为 22，以此类推。

#### 输入格式

输入包含多行，每行包含一个整数 NN。

当输入行为 N=0N=0 时，表示输入结束，且该行无需作任何处理。

#### 输出格式

对于每个输入整数 NN，输出一个满足要求的 NN 阶二维数组。

每个数组占 NN 行，每行包含 NN 个用空格隔开的整数。

每个数组输出完毕后，输出一个空行。

#### 数据范围

0≤N≤1000≤N≤100

#### 输入样例：

```
1
2
3
4
5
0
```

#### 输出样例：

```
1

1 1
1 1

1 1 1
1 2 1
1 1 1

1 1 1 1
1 2 2 1
1 2 2 1
1 1 1 1

1 1 1 1 1
1 2 2 2 1
1 2 3 2 1
1 2 2 2 1
1 1 1 1 1
```





坐标到左边 右边 上边 下边 最小值就是该值





```cpp
#include <iostream>

using namespace std;

int main() {
    int n;
    
    while(true) {
        cin >> n;
        if (!n) break;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int up = i + 1, down = n - i, left = j + 1, right = n - j;
                cout << min(min(up, down), min(left, right));
                if (j != n - 1) cout << " ";
            }
            cout << endl;
        }
        cout << endl;
        
    }
    
    return 0;
}
```







cpp 一定要赋初值



```cpp
int main() {
    // 这个空格不仅可以是空格 也可以是换行
    scanf("%d %d",1, 2);
}
```



防止科学输出



```cpp
#include <iostream>
#include <cmath>

using namespace std;

int main() {
    int n;
    
    while (true) {
        cin >> n;
        if (!n) return 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                cout << (long int)pow(2, i + j);
                if (j != n - 1) cout << " ";
            }
            cout << endl;
        }
        cout << endl;
    }
    
    return 0;
}
```





```cpp
int a[] = {1, 2, 3};
```



蛇形矩阵

```cpp
#include <iostream>

using namespace std;

int res[100][100];

int main() {
    int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0}, m, n;
    cin >> m >> n;
    for (int x = 0, y = 0, k = 1, d= 0; k <= m * n; k++) {
        res[x][y] = k;
        int a = x + dx[d], b = y + dy[d];
        if (a < 0 || a >= m || b < 0 || b >= n || res[a][b]) {
            d = (d + 1) % 4;
            a = x + dx[d];
            b = y + dy[d];
        }
        x = a;
        y = b;
        
    }
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            cout << res[i][j] << " ";
           
        }
        cout << endl;
    }
    return 0;
}
```

