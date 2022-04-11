

### vector

vector是变长数组，支持随机访问，不支持在任意位置O(1)插入。为了保证效率，元素的增删一般应该在末尾进行。



#### 基本使用

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    // 初始化
    vector<int> a({1, 2, 3});
    a = {1, 2, 3};
    
    // empty size clear
    cout << a.size() << endl;  
    cout << a.empty() << endl;
    cout << a.size() << endl;
    
    
    // 访问
    cout << a[0];
    
    // 迭代器 类似于指针
    cout << "iterator";
    vector<int>::iterator it = a.begin();
    while (it != a.end()) {
        cout <<  *it << endl;
        it++;
    }
    
    
    // front 第一个
    // back 最后一个
    cout << a.front() << endl;
    cout << a.back() << endl;
    
    // 从最后一个添加
    a.push_back(4);
    cout << a.back() << endl;
    // 删除最后一个
    a.pop_back();
   
    
   
    return 0;
}
```


```cpp

#include <iostream>
#include <vector>

using namespace std;

int main() {
    vector<int> a({1, 2, 3});
    vector<int> b = {1, 2, 3, 4};
    for (vector<int>::iterator it = a.begin(); it != a.end(); it++) {
        cout << *it;
    }
    cout << endl;
    
    for (auto it = b.begin(); it != b.end(); it++) cout << *it;
    
    return 0;
}
```







### queue

队列



```cpp
#include <iostream>
#include <queue>

using namespace std;

int main() {
   queue<int> q({1, 3, 4});
    // 错误的、
	//q = {1, 2};
   // push 压入 pop 弹出
   q.push(1);
   q.push(2);
  
    
   cout << q.size();
   q.pop();
   cout << q.size();
   
   cout << endl;
   // front 头 back 尾巴
   cout << q.front() << q.back();
    return 0;
}
```



priority_queue

```cpp
#include <iostream>
#include <queue>

using namespace std;

int main() {
    
    priority_queue<int> p;
    p.push(1);
    p.push(2);
    p.pop();
    // cout << p.size();
    
    cout << p.top();
    
    cout << "小根堆";
    priority_queue<int, vector<int>, greater<int>> q;
    // priority_queue<int, vector<int>, greater<int>> q
    q.push(10);
    q.push(2);
    cout << q.top();
  
    return 0;
}
```





```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

int main() {
    
    struct User {
        int a;
        bool operator> (const User& u) const {
            return a > u.a;
        }
    };
    priority_queue<int, vector<int>, greater<int>> p;
    p.push({1});
    p.push({2});
   cout << p.top();
    
    return 0;
}
```


```cpp
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

struct User {
    int x, y;
    bool operator< (const User &u) const {
        return x < u.x;
    }
};

// 前面和后面比较
bool cmp(User u1, User u2) {
   return u1.x > u2.x;
}

int main () {
    priority_queue<User> p;
    p.push({1, 2});
    p.push({10, 3});
    p.push({4, 3});
    
    // 注意运算符优先级
    cout << (p.top().x);
        
    
    return 0;
}
```




### stack





```cpp
#include <stack>
#include <iostream>

using namespace std;

int main() {
    
    stack<int> s;
    s.push(1);
    // top 返回空
    s.pop();
    
    cout << s.top();
    
    return 0;
}
```





### deque

双端队列deque是一个支持在两端高效插入或删除元素的连续线性存储空间。它就像是vector和queue的结合。与vector相比，deque在头部增删元素仅需要O(1)的时间；与queue相比，deque像数组一样支持随机访问。

```cpp
#include <iostream>
#include <deque>

using namespace std;

int main() {
    
  deque<int> d;
  // push_front push_back pop_front pop_back
  // front back
  // begin end
  // []
   d.push_front(1);
   d.push_front(2);
   d.push_back(3);
   d.pop_back();
   cout << d.front();
   cout << endl;
   for (auto it = d.begin(); it != d.end(); it++) cout << *it << ' ';    
    return 0;
}
```









### set



头文件set主要包括set和multiset两个容器，分别是“有序集合”和“有序多重集合”，即前者的元素不能重复，而后者可以包含若干个相等的元素。set和multiset的内部实现是一棵红黑树，它们支持的函数基本相同。



```cpp
#include <set>
#include <iostream>

using namespace std;

int main() {
    
    // empty size clear
    
    // begin end find insert
    // lower_bound upper_bound
    // erase count
  
    set<int> s1({1,2 ,-3});
    s1 = {1, 2, 3};
    set<int> s;
    s.insert(1);
    s.insert(1);
    s.insert(2);
    s.insert(34);
    s.insert(40);
    cout << s.size();
    cout << "find" << endl;
    cout << (s.find(3) == s.end());
    
    // it++ it--
    for (auto it = s.begin(); it != s.end(); it++) cout << *it;
    
    cout << endl;
    auto it = s.lower_bound(34);
    cout << "lower_bound" << *it;
    cout << endl;
    it = s.upper_bound(34);
    cout << "upper_bound" << *it;
    s.erase(it);
    cout << endl;
    cout << s.count(40);
    
    
    return 0;
}
```





### map



map容器是一个键值对key-value的映射，其内部实现是一棵以key为关键码的红黑树。Map的key和value可以是任意类型，其中key必须定义小于号运算符。





```cpp
#include <map>
#include <iostream>

using namespace std;


int main() {
    
    // empty size clear
    // []
    
    map<string, string> m1{{"a", "a1"}, {"b", "b1"}};
    m1 = {{"a", "b"}};
    map<int, int> m1;
    m1[1] = 2;
    m1.insert({1, 2});
    
    
    // begin end find insert
    cout << (m1.find(1) == m1.end()); 
    
    cout << endl;
    cout << "base" << m1.empty() << m1.size();
    m1.clear();
    cout << "clear";
    cout << m1.size();
    
    // begin end
    cout << "begin end" << endl;
    for (auto it = m1.begin(); it != m1.end(); it++) cout << (it->first) <<  (it -> second);
    
    return 0;
}

```









```cpp
// unordered_set 无序set o(1)
// unordered_multiset<int>

// unordered_map 一般使用这个 cpp 11支持


    pair<int, string> p, q;
    p = {1, "matt"};
    
    // cpp 99
    p = make_pair(1, "bb");
    cout << p.first << p.second;
```







```cpp
#include <iostream>
#include <bitset>
#include <algorithm>

using namespace std;

int main() {
   bitset<10> s;
   // 从后面
   s.set(3);
   cout << s;
   s.reset(3);
   cout << endl;
   cout << s;
    
    return 0;
}
```



```cpp
0000001000
0000000000
```

