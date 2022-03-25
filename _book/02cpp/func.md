## 理论



### 函数

#### 样例



```cpp
#include <iostream>

using namespace std;

// 函数声明
void f();

// 函数定义
// b有默认值不传也会有值
void f(int a, int b = 10) {
    // 静态变量 堆 
    // 无论调用多少次f 只会定义一次i
    static int i = 10;
    cout << "hello word";
    cout << a << b << endl;
}

int main() {
    // 不写返回值 会返回随机值
    f(1);
    f(2, 3);
    return 0;
}
```



#### 传递

```cpp
int max(int &x, int &y) {
    x = 1;
    y = 2;
    if (x > y) return x;
    return y;
}



```



```cpp
// *a a[] a[10]
void out(int *a) {
    for (int i = 0; i < 3; i++) cout << a[i];
    cout  << endl;
}
```



```cpp
// (*a)[3]
// 第一维参数可以省略 第二维不可以 a[][3]

void output(int a[3][3]) {
    
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) cout << a[i][j] << endl;
    }
}
```







#### 递归

```cpp
#include <iostream>

using namespace std;


int sum1(int n, int sum) {
    if (n == 0) return sum;
    else {
        sum = n + sum1(n - 1, sum);
    }
    
    return sum;
}
// main函数写在最后
int main() {
    cout << sum1(3, 0);
    return 0;
}


```





-x

 









## 习题





### 最小公倍数





```cpp
#include <iostream>

using namespace std;

int lcm(int a, int b) {
    if (a < b) swap(a, b);
    int i = 1;
    while (i != 0) {
        if (a * i % b == 0) break;
        i++;
    }
    return a * i;
}

int main() {
    int a, b;
    cin >> a >> b;
    cout << lcm(a, b);
    return 0;
}
```





### 817数组去重



给定一个长度为 n 的数组 a，请你编写一个函数：

```
int get_unique_count(int a[], int n);  // 返回数组前n个数中的不同数的个数
```

#### 输入格式

第一行包含一个整数 n。

第二行包含 n 个整数，表示数组 a。

#### 输出格式

共一行，包含一个整数表示数组中不同数的个数。

#### 数据范围

1≤n≤1000,
1≤ai≤1000

#### 输入样例：

```
5
1 1 2 4 5
```

#### 输出样例：

```
4
```











```cpp
#include <iostream>

using namespace std;

int unique(int n, int a[]) {
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        bool is_exists = false;
        for (int j = 0; j < i; j++) {
            if (a[j] == a[i]) {
                is_exists = true;
                break;
            }
        }
        if (!is_exists) cnt++;
    }
    return cnt;
}


int main() {
    int n, a[1000];
    cin >> n;
    for (int i = 0; i < n; i++) cin >> a[i];
    cout << unique(n, a);
    return 0;
}
```



### 823排列



给定一个整数 nn，将数字 1∼n1∼n 排成一排，将会有很多种排列方法。

现在，请你按照字典序将所有的排列方法输出。

#### 输入格式

共一行，包含一个整数 nn。

#### 输出格式

按字典序输出所有排列方案，每个方案占一行。

#### 数据范围

1≤n≤91≤n≤9

#### 输入样例：

```
3
```

#### 输出样例：

```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```





```cpp
#include <iostream>

using namespace std;

const int N = 10;
int n;

void dfs(int nums[], bool has_visited[], int index) {
    if (index > n) {
        for (int i = 1; i <= n; i++) cout << nums[i] << ' ';
        cout << endl;
        return;
    }
    
    for (int i = 1; i <= n; i++) {
        if (has_visited[i]) continue;
        has_visited[i] = true;
        nums[index] = i;
        dfs(nums, has_visited, index + 1);
        has_visited[i] = false;
    }
}

int main() {
    int nums[N];
    bool has_visited[N] = {false};
    cin >> n;
    dfs(nums, has_visited, 1);
    return 0;
}


```

