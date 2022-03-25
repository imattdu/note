+++
title = "init"
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

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <deque>
#include <set>
#include <map>

using namespace std;

int main() {
    vector<int> v1({1, 2, 3});
    v1 = {1, 2, 3};
    
    queue<int> q({1, 2, 3});
    // 错误
    //q = {1, 2, 3};
    
    deque<int> d1 = {1, 2, 3};
    
    stack<int> stk1({1, 2, 3});
    // 错误
    // stack<int> s = {1, 2, 3};
    
    
    set<int> s1 = {1, 2, 3};
    set<int> s2({1, 2, 3});
    
    map<string, string> m1({{1, 2}});
    
    return 0;
}

```


```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <deque>
#include <set>
#include <map>

using namespace std;

int main() {
   
    
    
   
    map<string, string> m1 = {
        {"name", "matt"}
    };
    
    m1.insert({"age", "17"});
    
    for (auto i = m1.begin(); i != m1.end(); i++) cout << i -> first;
    
    return 0;
}
```

```cpp
#include <iostream>

using namespace std;

int main() {
    // pair 在iostream里面
    pair<int, int> p1 = {1, 2};
    cout << p1.first;
    
    return 0;
}
```