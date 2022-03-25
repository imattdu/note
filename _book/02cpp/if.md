+++
title = "if"
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



<!--more-->









##  基础知识



### if





```cpp
#include <iostream>

using namespace std;

int main() {
    int i = 0;
    if (i == 0) cout i;
    else cout i + 1;
    
    if (i > 0) {
        i++;
        cout << i;
    }
    
    
    
    return 0;
}
```



### 关系表达式



- 大于 >
- 小于 <
- 大于等于 >=
- 小于等于 <=
- 等于 ==
- 不等于 !=





### 条件表达式

- 与 &&
- 或 ||
- 非 !





printf("%5d!\n", i);

左边补空格





printf("%-5d!\n", i);





printf("%05d!\n", i);





printf("%05.2f!\n", m);







```cpp
int t;
cin >> t;
cout << -t << endl;
```





|| 的优先级小于 &&





输入数据坑了

### 坐标

给定两个保留一位小数的浮点数 X,YX,Y，用来表示一个点的横纵坐标。

请你判断该点在坐标系中的位置。



#### 输入格式

共一行，包含两个浮点数 X,YX,Y，表示点的横纵坐标。

#### 输出格式

如果点在第一象限，则输出 `Q1`，在第二象限，则输出 `Q2`，以此类推。

如果点在原点处，则输出 `Origem`。

否则，如果点在 xx 坐标上，则输出 `Eixo X`，在 yy 坐标上，则输出 `Eixo Y`。

#### 数据范围

−10.0≤X,Y≤10.0−10.0≤X,Y≤10.0

#### 输入样例1：

```
4.5 -2.2
```

#### 输出样例1：

```
Q4
```

#### 输入样例2：

```
0.0 0.0
```

#### 输出样例2：

```
Origem
```





```cpp
#include <iostream>

using namespace std;

int main() {
    double a, b;
    cin >> a >> b;
    if (a > 0 && b > 0) {
        cout << "Q1";
    } else if (a < 0 && b > 0) {
        cout << "Q2";
    } else if (a < 0 && b < 0) {
        cout << "Q3";
    } else if (a > 0 && b < 0) {
        cout << "Q4";
    } else if (a == 0 && b == 0) {
        cout << "Origem";
    } else if (a == 0) {
        cout << "Eixo Y";
    } else if (b == 0) {
        cout << "Eixo X";
    }
    
    return 0;
}
```



### 游戏时间2



读取四个整数 A,B,C,DA,B,C,D，用来表示游戏的开始时间和结束时间。

其中 AA 和 BB 为开始时刻的小时和分钟数，CC 和 DD 为结束时刻的小时和分钟数。

请你计算游戏的持续时间。

比赛最短持续 11 分钟，最长持续 2424 小时。

#### 输入格式

共一行，包含四个整数 A,B,C,DA,B,C,D。

#### 输出格式

输出格式为 `O JOGO DUROU X HORA(S) E Y MINUTO(S)`，表示游戏共持续了 XX 小时 YY 分钟。

#### 数据范围

0≤A,C≤230≤A,C≤23,
0≤B,D≤590≤B,D≤59

#### 输入样例1：

```
7 8 9 10
```

#### 输出样例1：

```
O JOGO DUROU 2 HORA(S) E 2 MINUTO(S)
```

#### 输入样例2：

```
7 7 7 7
```

#### 输出样例2：

```
O JOGO DUROU 24 HORA(S) E 0 MINUTO(S)
```

#### 输入样例3：

```
7 10 8 9
```

#### 输出样例3：

```
O JOGO DUROU 0 HORA(S) E 59 MINUTO(S)
```





小时相等 分钟不等

10 9 10 7

这种情况忘记特殊处理





也可以化成min



```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int main() {
    int a, b, c, d;
    cin >> a >> b >> c >> d;
    if (a == c && b == d) {
        printf("O JOGO DUROU 24 HORA(S) E 0 MINUTO(S)");
    } else if (a == c) {
        if (d > b) printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)",c - a, d - b);
        else printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)", 24 + c - a - 1, 60 + d - b);
    } else if (c > a) {
        if (d >= b) printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)",c - a, d - b);
        else printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)", c - a - 1, 60 + d - b);
    } else {
        if (d >= b) printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)",24 + c - a, d - b);
        else printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)", 24 + c - a - 1, 60 + d - b);
    }
    return 0;
}
```









```cpp
#include <cstdio>
#include <iostream>

using namespace std;

int main() {
    int a, b, c, d;
    cin >> a >> b >> c >> d;
    int s = a * 60 + b;
    int e = c * 60 + d;
    int res = e - s;
    if (res <= 0) res += 1440;
    printf("O JOGO DUROU %d HORA(S) E %d MINUTO(S)", res / 60, res % 60);
    return 0;
}
```





### 简单的排序



读取三个整数并按升序对它们进行排序。

#### 输入格式

共一行，包含三个整数。

#### 输出格式

首先，将三个整数按升序顺序输出，每行输出一个整数。

然后，输出一个空行。

紧接着，将三个整数按原输入顺序输出，每行输出一个整数。

#### 数据范围

−100≤输入整数≤100−100≤输入整数≤100

#### 输入样例：

```
7 21 -14
```

#### 输出样例：

```
-14
7
21

7
21
-14
```



```cpp
#include <iostream>

using namespace std;

int main() {
    int a, b, c, a1, b1, c1, t;
    cin >> a >> b >> c;
    a1 = a;
    b1 = b;
    c1 = c;
    
    if (a1 < b1) {
        t = a1;
        a1 = b1;
        b1 = t;
        if (a1 < c1) {
            t = a1;
            a1 = c1;
            c1 = t;
        }
    }
    
    if (a1 < c1) {
        t = a1;
        a1 = c1;
        c1 = t;
        if (a1 < b1) {
            t = a1;
            a1 = b1;
            b1 = t;
        }
    }
    
    if (b1  < c1) {
        t = b1;
        b1 = c1;
        c1 = t;
    }
    
    cout << c1 << endl;
    cout << b1 << endl;
    cout << a1 << endl;
    
    cout << endl;
    cout << a << endl;
    cout << b << endl;
    cout << c << endl;
    
    
    return 0;
}
```

