+++
title = "字符串"
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



### 字符

每个常用字符都对应一个-128~127的数字，二者之间可以相互转化



常用ASCII值：’A’-‘Z’ 是65~90，’a’-‘z’是97-122，’0’-‘9’是48-57。

字符可以参与运算，运算时会将其当做整数：







```cpp
#include <iostream>

using namespace std;

int main() {
    char ch = 'a';
    cout << (int)ch << endl;
    ch++;
    cout << ch;
    return 0;
}
```







### 字符数组的使用

字符串就是字符数组加上结束符’\0’。

**可以使用字符串来初始化字符数组，但此时要注意，每个字符串结尾会暗含一个’\0’字符，因此字符数组的长度至少要比字符串的长度多1！**



#### 声明

```cpp
#include <iostream>

using namespace std;

int main() {
    // a0 这种一定要加\0
    char a0[10] = {'a', 'b', '\0'};
    // a1 a2自动加了\0
    char a1[10] = "hello";
    char a2[] = "aa";
    cout << a1 << endl << a2 << endl;
    cout << a0;
    return 0;
}
```

#### 输入输出

##### 基本的输入输出

```cpp
#include <iostream>

using namespace std;

int main() {
    char s[100];
    // 1
    cin >> s;
    cout << s;
    
    // 2
    char s1[100];
    scanf("%s", s);
    printf("%s", s);
    return 0;
    
     //3 注意的：将读入的字符串存到数组索引为1的地方
    char s2[100];
    scanf("%s", s2 + 1);
    cout << s2 + 1;
    return 0;
}
```



#### 读一行字符串

**fgets包含\n 不会过滤\n**



fgets 也是iostream里面的

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main() {
    char s1[100];
	// 1
    fgets(s1, 100, stdin);
    string s2;
    //2 getline 不能是字符数组
    getline(cin, s2);
    cout << s2;
    
    char s3[100];
    // 3
    cin.getline(s3, 100);
    
    // 4 
    puts(s3);
    return 0;
}
```



#### 字符数组常用的函数

cstring

- strlen(str)，求字符串的长度
- strcmp(a, b)，比较两个字符串的大小，a < b 返回-1，a == b 返回0，a > b返回1。这里的比较方式是字典序！
- strcpy(a, b)，将字符串b复制给从a开始的字符数组。



```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    char s1[100], s2[100];
    cin >> s1 >> s2;
    //1 字符串的长度
    cout << strlen(s1) << endl << strlen(s2);
    cout << endl;
    
    // 字符串比较
    cout << strcmp(s1, s2) << endl;
    
    // 3 s2 赋值给 s1
    strcpy(s1, s2);
    cout << s1 << s2;
    return 0;
}
```



字符串的遍历

```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    char s1[100];
    cin >> s1;
    
    for (int i = 0, len = strlen(s1); i < len; i++) cout << s1[i];
    return 0;
}
```





### string



#### 声明初始化



```cpp
#include <iostream>


using namespace std;

int main() {
    string s1 = "aaa";
    cout << s1;
    return 0;
}
```



#### 输入输出



```cpp
#include<iostream>
// #include <cstdio>

using namespace std;

int main() {
    string s1;
    // 1
    cin >> s1;
    cout << s1 << endl;
    
    // 2不能使用scanf 
    printf("%s", s1.c_str());
    
    puts(s1.c_str());
    
    
    string s2;
    //3 读一行
    getline(cin, s2);
    cout << s2;
    
    
    return 0;
}
```

#### 

#### 基本使用



- string的empty和size操作（注意size是无符号整数，因此 s.size() <= -1一定成立）
- 支持 > < >= <= == !=等所有比较操作，按字典序进行比较
- 字符串相加至少要有一个string 对象

```cpp
#include<iostream>
// #include <cstdio>

using namespace std;

int main() {
   
    
   // 1.1
    string str3 = "aa", str4;
    cout << str3.empty() << endl;
    cout << str4.empty() << endl;
    // 1.2
    cout << str3.size() << "size\n";
    
    // 2
    cout << (str3 < str4);
    
    str4 += str3;
    cout << str4;
    // 会报错 确保左右俩边至少有一个对象
    // "helo" "aa" 均为字面量
    string str5 = "helo" + "aa";
    return 0;
}
```



#### 范围遍历

auto可以自动判断类型

```cpp
#include <iostream>

using namespace std;

int main() {
    string s1 = "aaa bbb ccc";
    // 范围遍历
    for (auto &ch : s1) {
        cout << ch << endl;
        ch = 'a';
    }
    // auto 编译器自己去猜变量是什么类型
    cout << s1;
    return 0;
}
```





开始位置 几个字符

```cpp
substr(start, cout)
```



//删除最后一个字符

s1.pop_back()

// start len

s1.substr(0, 10)





## 习题





### 替换字符



给定一个由大小写字母构成的字符串。

把该字符串中特定的字符全部用字符 `#` 替换。

请你输出替换后的字符串。

#### 输入格式

输入共两行。

第一行包含一个长度不超过 3030 的字符串。

第二行包含一个字符，表示要替换掉的特定字符。

#### 输出格式

输出共一行，为替换后的字符串。

#### 输入样例：

```
hello
l
```

#### 输出样例：

```
he##o
```

#### **出错**



**// cin.getline 会计算\0**

```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    char s1[31], ch;
    // cin.getline 会计算\0
    cin.getline(s1, 31);
    cin >> ch;
    // cout << ch;
    
    for (int i = 0, len = strlen(s1); i < len; i++) {
        if (s1[i] == ch) s1[i] = '#';
    }
    cout << s1;
    
    return 0;
}
```





### 统计只出现一次的字符

给你一个只包含小写字母的字符串。

请你判断是否存在只在字符串中出现过一次的字符。

如果存在，则输出满足条件的字符中位置最靠前的那个。

如果没有，输出 `no`。

#### 输入格式

共一行，包含一个由小写字母构成的字符串。

数据保证字符串的长度不超过 100000100000。

#### 输出格式

输出满足条件的第一个字符。

如果没有，则输出 `no`。

#### 输入样例：

```
abceabcd
```

#### 输出样例：

```
e
```



#### **出错**



**没有想到这种方法**



```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    char s1[100000];
    cin >> s1;
    int c[26] = {0};
    
    for (int i = 0, len = strlen(s1); i < len; i++) c[s1[i] - 'a'] += 1;
    
    for (int i = 0, len = strlen(s1); i < len; i++) {
        if(c[s1[i] - 'a'] == 1){
            cout << s1[i];
            return 0;
        }
    }

    cout << "no";
    
    return 0;
}
```









































### 忽略大小写比较字符串



一般我们用 strcmpstrcmp 可比较两个字符串的大小，比较方法为对两个字符串从前往后逐个字符相比较（按 ASCII 码值大小比较），直到出现不同的字符或遇到 `\0` 为止。

如果全部字符都相同，则认为相同；如果出现不相同的字符，则以第一个不相同的字符的比较结果为准。

但在有些时候，我们比较字符串的大小时，希望忽略字母的大小，例如 `Hello` 和 `hello` 在忽略字母大小写时是相等的。

请写一个程序，实现对两个字符串进行忽略字母大小写的大小比较。

#### 输入格式

输入为两行，每行一个字符串，共两个字符串。注意字符串中可能包含空格。

数据保证每个字符串的长度都不超过 8080。

#### 输出格式

如果第一个字符串比第二个字符串小，输出一个字符 `<`。

如果第一个字符串比第二个字符串大，输出一个字符 `>`。

如果两个字符串相等，输出一个字符 `=`。

#### 输入样例：

```
Hello
hello
```

#### 输出样例：

```
=
```



#### 问题

1.没想到字符串全部转为小写

2.fgets() 需要我们过滤掉 \n



```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main() {
    char a[81], b[81];
    cin.getline(a, 81);
    cin.getline(b, 81);
    
    for (int i = 0, len = strlen(a); i < len; i++) {
        if (a[i] >= 'A' && a[i] <= 'Z') {
            a[i] = 'a' + a[i] - 'A';
        }
    }
    for (int i = 0, len = strlen(b); i < len; i++) {
        if (b[i] >= 'A' && b[i] <= 'Z') {
            b[i] = 'a' + b[i] - 'A';
        }
    }
    if (a[strlen(a) - 1] == '\n') a[strlen(a) - 1] = 0;
    if (b[strlen(b) - 1] == '\n') b[strlen(b) - 1] = 0;
    
    int t = strcmp(a, b);
    if (t == 0) puts("=");
    else if (t > 0) puts(">");
    else puts("<");
    
    
    
    return 0;
    
    
}
```





766去掉多余的空格



输入一个字符串，字符串中可能包含多个连续的空格，请将多余的空格去掉，只留下一个空格。

#### 输入格式

共一行，包含一个字符串。

#### 输出格式

输出去掉多余空格后的字符串，占一行。

#### 数据范围

输入字符串的长度不超过 200200。
保证输入字符串的开头和结尾没有空格。

#### 输入样例：

```
Hello      world.This is    c language.
```

#### 输出样例：

```
Hello world.This is c language.
```







```cpp
#include <iostream>

using namespace std;

int main() {
    string s;
    while(cin >> s) cout << s << ' ';
    return 0;
}
```







```cpp
#include <iostream>

using namespace std;

int main() {
    string s;
    getline(cin, s);
    string r;
    for (int i = 0; i < s.size(); i++) {
        if (s[i] != ' ') r += s[i];
        else {
            r += ' ';
            int j = i + 1;
            while (j < s.size() && s[j] == ' ') j++;
            // i++ 
            i = j - 1;
        }
    }
    cout << r.c_str();
    return 0;
}
```

### 767信息加密

在传输信息的过程中，为了保证信息的安全，我们需要对原信息进行加密处理，形成加密信息，从而使得信息内容不会被监听者窃取。

现在给定一个字符串，对其进行加密处理。

加密的规则如下：

1. 字符串中的小写字母，aa 加密为 bb，bb 加密为 cc，…，yy 加密为 zz，zz 加密为 aa。
2. 字符串中的大写字母，AA 加密为 BB，BB 加密为 CC，…，YY 加密为 ZZ，ZZ 加密为 AA。
3. 字符串中的其他字符，不作处理。

请你输出加密后的字符串。

#### 输入格式

共一行，包含一个字符串。注意字符串中可能包含空格。

#### 输出格式

输出加密后的字符串。

#### 数据范围

输入字符串的长度不超过 100100。

#### 输入样例：

```
Hello! How are you!
```

#### 输出样例：

```
Ifmmp! Ipx bsf zpv!
```





#### 出错

忘记考虑 z Z

```CPP
#include <iostream>

using namespace std;

int main() {
    string s1;
    getline(cin, s1);
    
    for (char &ch : s1) {
        if (ch >= 'a' && ch <= 'z') {
            ch =  (ch - 'a' + 1) % 26 + 'a';
        } else if (ch >= 'A' && ch <= 'Z') {
            ch =  (ch - 'A' + 1) % 26 + 'A';
        }
    }
    
    
    
    cout << s1;
    
    
    return 0;
}
```



### 输出字符串



给定一个字符串 aa，请你按照下面的要求输出字符串 bb。

给定字符串 aa 的第一个字符的 ASCII 值加第二个字符的 ASCII 值，得到 bb 的第一个字符；

给定字符串 aa 的第二个字符的 ASCII 值加第三个字符的 ASCII 值，得到 bb 的第二个字符；

…

给定字符串 aa 的倒数第二个字符的 ASCII 值加最后一个字符的 ASCII 值，得到 bb 的倒数第二个字符；

给定字符串 aa 的最后一个字符的 ASCII 值加第一个字符的 ASCII 值，得到 bb 的最后一个字符。

#### 输入格式

输入共一行，包含字符串 aa。注意字符串中可能包含空格。

数据保证字符串内的字符的 ASCII 值均不超过 6363。

#### 输出格式

输出共一行，包含字符串 bb。

#### 数据范围

2≤a的长度≤1002≤a的长度≤100

#### 输入样例：

```
1 2 3
```

#### 输出样例：

```
QRRSd
```









```cpp
#include <iostream>

using namespace std;

int main() {
    string a, b;
    getline(cin, a);
    for (int i = 0; i < a.size(); i++) 
        b += (char)(a[i] + a[(i + 1) % a.size()]);
        
    cout << b << endl;
    return 0;
}
```







sstream















### 770单词替换



输入一个字符串，以回车结束（字符串长度不超过 100100）。

该字符串由若干个单词组成，单词之间用一个空格隔开，所有单词区分大小写。

现需要将其中的某个单词替换成另一个单词，并输出替换之后的字符串。

#### 输入格式

输入共 33 行。

第 11 行是包含多个单词的字符串 ss;

第 22 行是待替换的单词 aa(长度不超过 100100);

第 33 行是 aa 将被替换的单词 bb(长度不超过 100100)。

#### 输出格式

共一行，输出将 ss 中所有单词 aa 替换成 bb 之后的字符串。

#### 输入样例：

```
You want someone to help you
You
I
```

#### 输出样例：

```
I want someone to help you
```



```cpp
#include <iostream>
#include <sstream>

using namespace std;

int main() {
    string s, a, b;
    getline(cin, s);
    cin >> a >> b;
    stringstream ssin(s);
    string str;
    while(ssin >> str) {
        if (str == a) cout << b << ' ';
        else cout << str << ' ';
    }
   
    
    return 0;
}
```







```cpp
#include <iostream>
#include <sstream>

using namespace std;

int main() {
    string s;
    getline(cin, s);
    stringstream aa(s);
    int a, b;
    string c;
    double d;
    aa >> a >> b >> c >> d;
    cout << a << endl << b << endl << c << endl << d;
   
    
    return 0;
}
```



```cpp
1 2 aaa 1.1
```







```cpp
#include <iostream>
#include <sstream>
#include <cstdio>

using namespace std;

int main() {
    char s[1000];
    fgets(s, 1000, stdin);
    int a, b;
    char c[100];
    double d;
    sscanf(s, "%d%d%s%ld", &a, &b, c, &d);
   
    printf("%d %d %s %ld", a, b, c, d);
    
    return 0;
}
```

### 777最长出现的连续字符

求一个字符串中最长的连续出现的字符，输出该字符及其出现次数，字符串中无空白字符（空格、回车和 tabtab），如果这样的字符不止一个，则输出第一个。

#### 输入格式

第一行输入整数 NN，表示测试数据的组数。

每组数据占一行，包含一个不含空白字符的字符串，字符串长度不超过 200200。

#### 输出格式

共一行，输出最长的连续出现的字符及其出现次数，中间用空格隔开。

#### 输入样例：

```
2
aaaaabbbbbcccccccdddddddddd
abcdefghigk
```

#### 输出样例：

```
d 10
a 1
```













```cpp
#include <iostream>

using namespace std;

int main() {
    int n;
    string s;
    cin >> n;
    while (n--) {
        cin >> s;
        int cnt = 0;
        char ch;
        for (int i = 0; i < s.size(); i++) {
            int j = i;
            while (j < s.size() && s[j] == s[i]) j++;
            if (j - i > cnt) {
                cnt = j - i;
                ch = s[i];
            }
            j = j - 1;
            i = j;
        }
        cout << ch << ' ' <<  cnt << endl;
    }
    return 0;
    
}

```



### 最长单词



一个以 `.` 结尾的简单英文句子，单词之间用空格分隔，没有缩写形式和其它特殊形式，求句子中的最长单词。

#### 输入格式

输入这个简单英文句子，长度不超过 500500。

#### 输出格式

该句子中最长的单词。如果多于一个，则输出第一个。

#### 输入样例：

```
I am a student of Peking University.
```

#### 输出样例：

```
University
```

```cpp
#include <iostream>

using namespace std;

int main() {
    string s, res = "";
    while(cin >> s) {
        // back 最后一个字符
        if (s.back() == ' ') s.pop_back();
        if (s.size() > res.size()) res = s;
    }
    puts(res.c_str());
    
    return 0;
}
```



### 775倒排单词



编写程序，读入一行英文(只包含字母和空格，单词间以单个空格分隔)，将所有单词的顺序倒排并输出，依然以单个空格分隔。

#### 输入格式

输入为一个字符串（字符串长度至多为 100100）。

#### 输出格式

输出为按要求排序后的字符串。

#### 输入样例：

```
I am a student
```

#### 输出样例：

```
student a am I
```





#### 思路

不用读取整行 一个字符串读取就可以



```cpp
res = s + ' ' + res;
```





#### code

```cpp
#include <iostream>

using namespace std;

int main() {
    string s, res;
    while(cin >> s)  res = s + ' ' + res;
    cout << res;
    return 0;
}
```









### 776字符串移位包含问题

对于一个字符串来说，定义一次循环移位操作为：将字符串的第一个字符移动到末尾形成新的字符串。

给定两个字符串 s1s1 和 s2s2，要求判定其中一个字符串是否是另一字符串通过若干次循环移位后的新字符串的子串。

例如 `CDAA` 是由 `AABCD` 两次移位后产生的新串 `BCDAA` 的子串，而 `ABCD` 与 `ACBD` 则不能通过多次移位来得到其中一个字符串是新串的子串。

#### 输入格式

共一行，包含两个字符串，中间由单个空格隔开。

字符串只包含字母和数字，长度不超过 3030。

#### 输出格式

如果一个字符串是另一字符串通过若干次循环移位产生的新串的子串，则输出 `true`，否则输出 `false`。

#### 输入样例：

```
AABCD CDAA
```

#### 输出样例：

```
true
```

#### 思路

遍历移位次数 -> 遍历start -> 遍历每个字符看是否相等

#### code

```cpp
#include <iostream>

using namespace std;

int main() {
    string a, b;
    cin >> a >> b;
    if (a.size() < b.size()) swap(a, b);
    for (int i = 0; i < a.size(); i++) {
        a = a.substr(1) + a[0];
        for (int j = 0; j + b.size() <= a.size(); j++) {
            int k = 0;
            for (; k < b.size(); k++) {
                if (b[k] != a[j + k]) break;
            }
            if (k == b.size()) {
                puts("true");
                return 0;
            }
        }
        
    }
    puts("false");
    return 0;
}
```





### 777字符串的乘方





给定两个字符串 aa 和 bb，我们定义 a×ba×b 为他们的连接。

例如，如果 a=a=`abc` 而 b=b=`def`， 则 a×b=a×b=`abcdef`。

如果我们将连接考虑成乘法，一个非负整数的乘方将用一种通常的方式定义：a0=a0=``(空字符串)，a(n+1)=a×(an)a(n+1)=a×(an)。

#### 输入格式

输入包含多组测试样例，每组测试样例占一行。

每组样例包含一个字符串 ss，ss 的长度不超过 100100。

最后的测试样例后面将是一个点号作为一行。

#### 输出格式

对于每一个 ss，你需要输出最大的 nn，使得存在一个字符串 aa，让 s=ans=an。

#### 输入样例：

```
abcd
aaaa
ababab
.
```

#### 输出样例：

```
1
4
3
```



#### 思路

len / n = m

r = m * n

r == s



#### code

```cpp
#include <iostream>

using namespace std;

int main() {
    string str, s;
    while(cin >> str, str != ".") {
        int len = str.size();
        for (int n  = len; n; n--) {
            if (len % n == 0) {
                int m = len / n;
                s = str.substr(0, m);
                string r;
                for (int i = 0; i < n; i++) r += s;
                if (r == str){
                    cout << n << endl;
                    break;
                } 
            }
        }
    }
    
    return 0;
}
```



### 778字符串最大跨距



有三个字符串 S,S1,S2S,S1,S2，其中，SS 长度不超过 300300，S1S1 和 S2S2 的长度不超过 1010。

现在，我们想要检测 S1S1 和 S2S2 是否同时在 SS 中出现，且 S1S1 位于 S2S2 的左边，并在 SS 中互不交叉（即，S1S1 的右边界点在 S2S2 的左边界点的左侧）。

计算满足上述条件的最大跨距（即，最大间隔距离：最右边的 S2S2 的起始点与最左边的 S1S1 的终止点之间的字符数目）。

如果没有满足条件的 S1S1，S2S2 存在，则输出 −1−1。

例如，S=S= `abcd123ab888efghij45ef67kl`, S1=S1= `ab`, S2=S2= `ef`，其中，S1S1 在 SS 中出现了 22 次，S2S2 也在 SS 中出现了 22 次，最大跨距为：1818。

#### 输入格式

输入共一行，包含三个字符串 S,S1,S2S,S1,S2，字符串之间用逗号隔开。

数据保证三个字符串中不含空格和逗号。

#### 输出格式

输出一个整数，表示最大跨距。

如果没有满足条件的 S1S1 和 S2S2 存在，则输出 −1−1。

#### 输入样例：

```
abcd123ab888efghij45ef67kl,ab,ef
```

#### 输出样例：

```
18
```







#### 思路

一个一个字符读入



#### code

```cpp
#include <iostream>

using namespace std;

int main() {
    string str, s, s1, s2;
    char c;
    while (cin >> c, c != ',') s += c;
    while (cin >> c, c != ',') s1 += c;
    while (cin >> c) s2 += c;
    
    int l = 0, r = s.size() - s2.size();
    while (l <= s.size() - s1.size()) {
        int i = 0;
        while(i < s1.size() && s1[i] == s[l + i]) i++;
        if (i == s1.size())break;
        l++;
    }
    
    while(r >= 0) {
        int i = 0;
        while (i < s2.size() && s2[i] == s[r + i]) i++;
        if (i == s2.size()) break;
        r--;
    }
    
   
    l = l + s1.size() - 1;
    if (l <= r) {
        cout << r - l - 1;
        return 0;
    }
    cout << -1;
    return 0;
}
```



### 779 最长公共字符串后缀

给出若干个字符串，输出这些字符串的最长公共后缀。

#### 输入格式

由若干组输入组成。

每组输入的第一行是一个整数 N。

N 为 0 时表示输入结束，否则后面会继续有 N 行输入，每行是一个字符串（字符串内不含空白符）。

每个字符串的长度不超过 200。

#### 输出格式

共一行，为 NN 个字符串的最长公共后缀（可能为空）。

#### 数据范围

1≤N≤200

#### 输入样例：

```
3
baba
aba
cba
2
aa
cc
2
aa
a
0
```

#### 输出样例：

```
ba

a
```

#### 思路

获取长度最小的字符串 

len -> 0 {

​	遍历字符串 {

​		遍历每个后缀字符

​	}

}



#### code

```cpp
#include <iostream>

using namespace std;

const int N = 200;


int main() {
    int n;
    while(cin >> n, n) {
        string s[200];
        int len = 200;
        for (int i = 0; i < n; i++) {
            cin >> s[i];
            if (s[i].size() < len) len = s[i].size();
        }
        while (len) {
            bool success = true;
            for (int i = 1; i < n; i++) {
                bool is_same = true;
                for (int j = 1; j <= len; j++) {
                    if(s[0][s[0].size() - j] != s[i][s[i].size() - j]) {
                        is_same = false;
                        break;
                    }
                }
                if (!is_same) {
                    success = false;
                    break;
                }
            }
            if (success) break;
            len--;
        }
        cout << s[0].substr(s[0].size() - len) << endl;
    }
    
    return 0;
}
```

