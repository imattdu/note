

## 概念



### 类





```cpp
#include <iostream>

using namespace std;

class Solution {
    
public:
    // 方法
    int add(int i, int j) {
        return i + j;
    }
    int age = 11;
    // 构造函数
    Solution() {}
    A(int _age, string _name) : age(_age), name(_name){}
    // Solution(int _age) : age(_age){}

    Solution(int _age) {
        age = _age;
    }
        
        
// 声明几个变量
}solution1, solution2, solution3[10];

int main() {
    // 变量 . 访问
    Solution s = Solution(23);
    // s.age = 12;
    cout << s.age;
    cout << endl;
    // 指针 -> 进行访问
    Solution* s1 = new Solution();
    s1 -> age = 13;
    cout << s1 -> age;
    
    return 0;
}

```





类中的变量和函数被统一称为类的成员变量。

private:类的外部不可以访问

public:类的外部可以访问

类结束加**；**



都是public不用写构造器，直接{} 否则需要写构造器







### 结构体

```cpp
struct Book {

    int age = 11;
    string name;
};

int main() {
    
    Book b = Book();
    cout << b.name;
    Book b1 = {11, "matt"};
    
    return 0;
}
```



**结构体的用法和类一模一样，不同的是类默认是private,结构体是public**





### 指针





```cpp
#include <iostream>

using namespace std;

int main() {
    
    int a = 10;
    // 指针的声明
    // &a 获取a的地址
    int p = &a;
    // int* p = &a; 
    // int& p = a; p相当于a的别名
    cout << p << endl;
    // 修改p的值
    *p = 12;
    // *p取得变量的值
    cout << *p;
    return 0;
}
```





### 链表



```cpp
#include <iostream>

using namespace std;

struct Node {
  int val;
  Node* next;
  Node(){}
  Node(int _val) : val(_val){}
  
}*head;

int main() {
    for (int i = 1; i < 5; i++) {
        // Node() 变量   new Node(i) 指针
        Node* p = new Node(i);
        //指针 -> 变量.
        p -> next = head;
        head = p;
    }
    Node* p = head;
    while (p) {
        cout << p -> val << ' ';
        p = p -> next;
    }
    
    return 0;
}
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/202112042259433.png)





栈从上往下 随机的



堆从下往上 会有默认值





## 习题

### 84求1+2+...+n

求 1+2+…+n，要求不能使用乘除法、for、while、if、else、switch、case 等关键字及条件判断语句 (A?B:C)。

#### 样例

```
输入：10

输出：55
```

#### 思路

**巧妙的使用了&& 的短路功能**

#### code

```cpp
class Solution {
public:
    int getSum(int n) {
        int res = 0;
        //  return 不能放在条件表达式里面
        n > 0 && (res += getSum(n - 1) + n);
        return res;
    }
    
};
```





### o1时间删除节点







给定单向链表的一个节点指针，定义一个函数在O(1)时间删除该结点。

假设链表一定存在，并且该节点一定不是尾节点。

#### 样例

```
输入：链表 1->4->6->8
      删掉节点：第2个节点即6（头节点为第0个节点）

输出：新链表 1->4->8
```

#### 思路

**整体赋值**



#### code

```cpp
class Solution {
public:
    void deleteNode(ListNode* node) {
        // node -> val = node -> next -> val;
        // node -> next = node -> next -> next;
        // 整体赋值
        *node = *(node -> next);
    }
};
```













### 合并俩个有序链表

输入两个递增排序的链表，合并这两个链表并使新链表中的结点仍然是按照递增排序的。

#### 样例

```
输入：1->3->5 , 2->4->5

输出：1->2->3->4->5->5
```



#### 思路



**tail = tail -> next = l1;**

等价于

tail -> nest = l1;

tail = tail -> next;



#### code

##### 垃圾代码

```cpp
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        if (!l1) return l2;
        else if (!l2) return l1;
        ListNode* head = new ListNode(-1), *node = new ListNode(-1);
        if (l1 -> val < l2 -> val) head -> next = l1;
        else head -> next = l2;
        node = head;
        while (l1 && l2) {
            if (l1 -> val < l2 -> val) {
                node -> next = l1;
                l1 = l1 -> next;
            } else {
                node -> next = l2;
                l2 = l2 ->  ;
            }
            node = node -> next;
        }
        
        if (l1) node -> next = l1;
            
        if (l2) node -> next = l2;
        return head -> next;
    
    }
};
```





##### 优秀代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(-1), tail = dummy;
        while(l1 && l2) {
            if (l1 -> val < l2 -> val) {
                tail = tail -> next = l1;
                l1 = l1 -> next;
            } else {
                tail = tail -> next = l2;
                l2 = l2 -> next;
            }
        }
        
        if (l1) tail -> next = l1;
        if (l2) tail -> next = l2;
        return dummy -> next;
        
    }
};
```













### 翻转链表

#### 思路

声明俩个节点 p q

##### 垃圾代码



```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* node = new ListNode(-1);
        ListNode* next;
        while(head) {
            next = head -> next;
            head -> next = node -> next;
            node -> next = head;
            head = next;
        } 
        return node ->  next;
    }
};


```





##### 优秀代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head -> next) return head;
        
        auto p = head, q = head -> next;
        while(q) {
            auto o = q -> next;
            q -> next = p;
            p = q, q = o;
        }
        head -> next = NULL;
        return p;
        
    }
};
```



##### 递归

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head -> next) return head;
        auto res = reverseList(head -> next);
        head -> next -> next = head;
        head -> next = NULL;
        return res;
        
    }
};
```







### 俩个链表找公共的节点



**空节点**

**0 NULL nullptr**





#### 思路





#### code









```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *findFirstCommonNode(ListNode *headA, ListNode *headB) {
        auto l1 = headA;
        auto l2 = headB;
        
        while(l1 != l2) {
            if (l1) l1 = l1 -> next;
            else l1 = headB;
            if (l2) l2 = l2 -> next;
            else l2 = headA;
        }
        
    
        return l1;
        
    }
};
```





### 删除重复的节点

1-> 2-> 2-> 3-> 3





1->3

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplication(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy -> next = head;
        auto p = dummy;
        while (p -> next) {
            auto q = p -> next;
            while(q -> next && q -> val == q -> next -> val) q = q -> next;
            
     
            if (p -> next != q)  {
                p -> next = q -> next;
            } else {
                p = q;
            }
            
            
        }
        
        return dummy -> next;
        
    }
};
```

