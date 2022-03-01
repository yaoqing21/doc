





# 语法基础

## 1.变量

变量需要先定义，后使用。

```c++
int a = 3;    // 定义变量
```

常见的变量类型

```c++
1.整型
int    // 4 Bytes，取值范围-2^31 ~ 2^31 - 1
long long    // 8 Bytes, 取值范围 -2^63 ~ 2^63 - 1
2.浮点型
float    // 4 Bytes, 6 - 7 位有效数字
double    // 8 Bytes, 15 - 16 位有效数字
3.字符型
char    // 1 Byte
4.布尔型
bool    // 1 Byte
```

## 2.输入输出

`cin和cout`对应头文件`#include<iostream>`;`scanf和printf`对应头文件`#include<cstdio>`。

特殊转义字符`%%`表示`%`。

常识性判断：

```c++
// 判断能否构成三角形
if((a + b) > c &&  (b + c) > a && (a + c) > b)
// 判断闰年
if(year % 400 == 0 || (year % 4 == 0 && year % 100 != 0))
```

## 3.循环

- ```c++
  for(  ;  ;  )
  {
      循环体
  }
  ```

- ```c++
  while(  )
  {
      循环体
  }
  ```

- ```c++
  do
  {
      循环体
  }while(  )
  ```

曼哈顿距离: 给定两个点(x1, y1) , (x2, y2),两点之间的曼哈顿距离为`distance = |x1 - x2| + |y1 - y2|`

应用：给定一个坐标原点，那么离这个原点曼哈顿距离相等的点，以它为中心呈菱形散开。

## 4.数组

1. `memset(a, 0, sizeof a)`表示将a中n个字节的值赋值为0,这个函数常用于给数组赋值。`memcpy(b, a, sizeof a)`常用来将a数组拷贝至b数组。

- 

```c++
#include<bits/stdc++.h>

using namespace std;

/* 画出如下矩阵
1 1 1
1 2 1
1 1 1
*/
int main()
{
    int n;
    while(cin >> n && n)
    {
        for(int i = 1; i <= n; i ++)
        {
            for(int j = 1; j <= n; j ++)
            {
                int up = i, down = n - i + 1, left = j, right = n - j + 1;
                int ans = min(min(up, down), min(left, right));
                cout << ans << " ";
            }
            cout << endl;
        }
        cout << endl;
    }
    return 0;
}
```

- 蛇形矩阵

```c++
#include<bits/stdc++.h>
using namespace std;
/* 画出如下蛇形矩阵
1 2 3
8 9 4
7 6 5
*/
int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
int A[105][105];
int main()
{
    int n, m;
    cin >> n >> m;
    int x = 0, y = 0;
    int d = 0;
    for(int i = 1; i <= n * m; i ++)
    {
        A[x][y] = i;
        int xx = x + dir[d][0], yy = y + dir[d][1];
        if(xx < 0 || xx >= n || yy < 0 || yy >= m || A[xx][yy])
        {
            d = (d + 1) % 4;
            xx = x + dir[d][0];
            yy = y + dir[d][1];
        }
        x = xx, y = yy;
    }
    
    for(int i = 0; i < n; i ++)
    {
        for(int j = 0; j < m; j ++)
        {
            cout << A[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

## 5.字符串

| 常见字符 | ' '(空格) | '0'  | 'A'  | 'a'  |
| -------- | --------- | ---- | ---- | ---- |
| ASCII值  | 32        | 48   | 65   | 97   |

```c++
// char类型字符串读入
char s[100];
scanf("%s", s);
fgets(s, 100, stdin); // 读入整行，包括空格,注意：会将行末的回车读入！
scanf("%s", s + 1);  // 从s[1]开始读入
// string类型字符串读入
string s;
cin >> s;
getline(cin, s);  // 读入整行，包括空格，不会读入回车
// char类型字符串输出
printf("%s", s);
puts(s);  // 相当于printf输出，再加上一个换行 
// string类型字符串输出
cout << s;
printf("%s", s.c_str());
puts(s.c_str());
```

> **易错点**：字符串等于字符数组的长度加1， 后面有一个'\0'。

```c++
// char类型字符数组
// 头文件#include <string.h>，包含在#include<iostream>里。
// 常用函数：
char s[100], t[100];
strlen(s);  // 求字符数组s的长度，不包括'\0'
strcmp(s, t);   // 比较两个字符数组的字典序，'=='时，返回0；'<'时，返回-1；'>'时，返回1
strcpy(t, s);   // 将字符串s复制给t
```

> 字典序：依次比较两个字符串中的对应字符的ASCII值的大小， 若相当，则往后顺延， 继续比较。

```c++
// string类型的可变长字符序列
// 头文件#include<string>
// 常用函数
string s;
s.size();  //  求字符序列s的长度
s.empty();  //  判断s是否为空字符序列
s.substr(pos, len)  // 返回一个string类型的字符串，包含s中从pos位置开始的长度为len的子串
// 支持 > < >= <= == !=等所有比较操作，按字典序进行比较。
// 做加法运算时，字面值和字符都会被转化成string对象，因此直接相加就是将这些字面值串联起来;当把string对象和字符字面值及字符串字面值混在一条语句中使用时，必须确保每个加法运算符的两侧的运算对象至少有一个是string
string s4 = s1 + “, “;	// 正确：把一个string对象和有一个字面值相加
string s5 = “hello” +”, “; // 错误：两个运算对象都不是string

string s6 = s1 + “, “ + “world”;  // 正确，每个加法运算都有一个运算符是string
string s7 = “hello” + “, “ + s2;  // 错误：不能把字面值直接相加，运算是从左到右进行的
```

- 双指针用法

```c++
// 题目描述:输入一个字符串，字符串中可能包含多个连续的空格，请将多余的空格去掉，只留下一个空格。
#include <iostream>

using namespace std;

int main()
{
    string s;
    getline(cin, s);

    string r;
    for (int i = 0; i < s.size(); i ++ )  // 双指针i, j
        if (s[i] != ' ') r += s[i];
        else
        {
            r += ' ';
            int j = i;
            while (j < s.size() && s[j] == ' ') j ++ ;
            i = j - 1;
        }
    cout << r << endl;

    return 0;
}
```

## 6.类、结构体、指针和引用

> 内存主要分为**堆空间和栈空间**，堆空间存放全局变量和静态变量，栈空间存放局部变量和函数。堆空间变量地址分配从小到大，栈空间变量地址分配从大到小。

- 结构体：供用户自定义数据类型

```c++
// 结构体定义
struct Student
{	
	// 成员列表
	string name;
	int age;
	int score;
}stu3;
```

- 创建结构体变量的三种方式: 

1. `struct 结构体名 变量名; // 创建结构体变量时，struct关键字可以省略`
2. `struct 结构体名 变量名 = {成员初始值,...};`
3.  `定义结构体时顺便创建变量`

- 结构体数组

```c++
struct Student arr[3] = {
	{"zhangsan", 18, 60},
	{"lisi", 19, 70},
	{"wangwu", 20, 59}
};
for(int i = 0; i < 3; i ++)
   {
      cout << "name = " << stu[i].name
           << " age = " << stu[i].age
           << " score = " << stu[i].score << endl;
   }
```

- 结构体指针

```c++
struct Student s = {"zhangsan", 18, 60};
Student *p = s;
cout << p->name << p->age >> p->score;
```

- 链表

```c++
struct ListNode {
	int val;
	ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}  // 构造函数
};
=======================================================
#include<iostream>
using namespace std;
struct Node
{
    int val;
    struct Node *next;
    Node(int _val)
    {
        val = _val;
        next = NULL;
    }
};
int main()
{
    Node *n1 = new Node(1);
    Node *n2 = new Node(2);
    Node *n3 = new Node(3);
    
    n1->next = n2;
    n2->next = n3;
    
    Node *p = n1;
    while(p != NULL)  // 遍历链表
    {
        cout << p->val << " ";
        p = p->next;
    }
    return 0;
}
```

1. 删除当前节点(将下一节点复制至当前节点，然后删除下一个节点，即相当于删除当前节点)

```c++
class Solution {
public:
    void deleteNode(ListNode* node) {
        ListNode *p = node->next;
        node->val = node->next->val;
        node->next = node->next->next;
        delete p;
    }
};
```

2.合并链表

```c++
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode *head = new ListNode(-1);
        ListNode *tail = head;
        while(l1 != NULL && l2 != NULL)
        {
            if(l1->val < l2->val)
            {
                tail->next = l1;
                tail = tail->next;
                l1 = l1->next;
                tail->next = NULL;
            }
            else 
            {
                tail->next = l2;
                tail = tail->next;
                l2 = l2->next;
                tail->next = NULL;
            }
        }
        if(l1)  tail->next = l1;
        if(l2)  tail->next = l2;
        return head->next;
    }
};
```

3.反转链表

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
// 递归版本
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(!head || !head->next)
        return head;
        ListNode *tail = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return tail;
    }
};
================================================
// 迭代版本
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(!head || !head->next)
        return head;
        ListNode *tail = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return tail;
    }
};
```

4.删除链表中重复的节点

```c++
/* 题目意思：在一个排序的链表中，存在重复的节点，请删除该链表中重复的节点，重复的节点不保留。
样例：
输入：1->2->3->3->4->4->5

输出：1->2->5
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* head) {
        if(!head || !head->next)  // 链表为空或者只存在头结点时，不需要去除重复，直接返回头结点
        return head;
        ListNode *dummy = new ListNode(-1);  // 构造一个虚拟结点来便于后续删除结点
        dummy->next = head;  // 将构造出来的结点指向原链表
        ListNode *pre = dummy, *cur = head;
        while(cur)
        {
            while(cur->next && cur->val == cur->next->val)
            cur = cur->next;
            if(pre->next == cur)
            pre = pre->next;
            else pre->next = cur->next;
            cur = cur->next;
        }
        return dummy->next;  // 返回删除重复元素后链表的第一个结点的指针
    }
};
```

## 7.STL

7.1`vector`

vector可以视为可以动态扩容的数组，支持随机访问。

```c++
#include<vector>  // 头文件
vector<int> vec;  // 初始化一个存放int类型数据的vector容器
vector<int> v[100];  // 第一维为100,第二位为可变数组;相当于二维数组

struct Rec
{
	int x, y;
};
vector<Rec> c;  // vector可以存放自定义类型

// 常用函数
vec.size();  // 返回vector容器中元素的个数,时间复杂度O(1)
vec.empty();  // 判断vector容器是否为空，如果为空，返回false;否则,返回true
vec.front()  // 取vector容器中第一个元素，等价于vec[0], *vec.begin()
vec.back()  //取vector容器中的最后一个元素，等价于vec[vec.size() - 1]

//vector容器元素的遍历
for(int i = 0; i < vec.size(); i ++)
cout << vec[i] << " ";
-----------------------------------
for(vector<int>::iterator it = vec.begin(); it != vec.end(); it ++)
cout << *it << " ";

//常用操作
vec.push_back(x)  // 将元素x添加在vec的尾部
vec.pop_back()  // 将vec中最后一个元素弹出,没有返回值！！！
```

7.2`queue`

queue用来处理先进先出的逻辑，元素只能在队尾插入，从队头删除。

```c++
#include<queue>  // 头文件queue主要包括普通队列queue和优先队列priority_queue两个容器
queue<int> q;

struct Rec
{
	int x, y;
};
queue<Rec> a;

// 常用函数
q.push(x)  // 将元素x插入队尾
q.pop();    // 将队头元素弹出
q.front();  // 查询队头元素
q.back();  // 查询队尾元素
// 简单遍历
while(!q.empty())
    {
        cout << q.front() << " ";
        q.pop();
    }
=================================================================================
priority_queue<int> q1;  // 默认是大根堆，只支持基本类型
priority_queue<int, vector<int>, greater<int> > q2;  // 小根堆
priority_queue<Rec> q3;  // 对于用户自定义类型，定义大根堆时必须重载小于号；定义小根堆时必须重载大于号
// 重载小于号
struct Rec
{
    int x, y;
    bool operator < (const Rec &t) const
    {
        return x < t.x; // 当x < t.x为真时，x结构体小于t结构体，意思是成员x越小时，结构体排在后面，这本身是一个大根堆，会优先弹出排序后大的结构体，所以重载小于号后为大根堆。注意：会跟字面上的意思相反，根据(x < t.x)这个布尔表达式来排序，但是本身是大根堆，会优先弹出排序后大的结构体。
//      return x > t.x;  表示小根堆
    }
}

// 常用操作
q1.push(x);  // 把元素x加入优先队列
q1.pop();  // 弹出优先队列队头元素
q1.top();  // 查询优先队列队头元素
```

- `stack`

具有后进先出特性的数据结构

```c++
#include<stack>  //头文件
stack<int> st;

// 常用函数
st.empty();
st.size();
st.push(x);  // 向栈顶添加元素x
st.pop();  // 弹出栈顶元素
st.top();  // 返回栈顶元素
```

- `deque`

双端队列`deque`是一个支持在两端高效插入或删除元素的连续线性存储空间。它就像是`vector`和`queue`的结合。与`vector`相比，`deque`在头部增删元素仅需要`O(1)`的时间；与`queue`相比，`deque`像数组一样支持随机访问。

```c++
#include<deque>  // 头文件
deque<int> deq;
deq[0]  // 随机访问
deq.begin() / deq.end();  返回deque的头尾迭代器
deq.front();  // 获取队头元素
deq.back();  //  获取队尾元素
deq.push_back(x);  // 从队尾添加元素x
deq.push_front(x);  // 从队头添加元素x
deq.pop_back();  // 将队尾元素弹出
deq.pop_front();  // 将队头元素弹出
deq.clear();  // 清空deque队列
```

- `set`

头文件set主要包括`set`和`multiset`两个容器，分别是“有序集合”和“有序多重集合”，即前者的元素不能重复，而后者可以包含若干个相等的元素。`set`和`multiset`的内部实现是一棵红黑树，它们支持的函数基本相同。

```c++
#include<set>  // 头文件

set<int> s;
struct Rec
{
	int x, y;
	bool operator < (const Rec &t) const
	{
		return x < t.x;
	}
};
set<Rec> s;  // set默认是升序排列，自定义类型时需要重载运算符

// 常用函数
s.clear();  // 清空 set 容器中所有的元素，即令 set 容器的 size() 为 0。
s.empty();  // 若set容器为空，则返回 true；否则 false。
s.size();  // 返回当前 set 容器中存有元素的个数。
s.count(x);  // 返回集合s中等于x的元素个数，时间复杂度为 O(k +logn)，其中k为元素x的个数。

// 迭代器
set和multiset的迭代器成为“双向访问迭代器”，不支持“随机访问”，支持星号“*”解引用，同时支持“++”和“--”两种算术操作。若把it++，则it会指向“下一个”元素。这里的“下一个”元素是指在元素从小到大排序的结果中，排在it下一名的元素。同理，若把it--，则it将会指向排在“上一个”的元素。
s.begin();  // 返回指向集合中第一个元素的迭代器
s.end();  //  返回集合中最大元素的下一个位置的迭代器
s.insert();  // 把元素x插入集合s,时间复杂度为O(logn)
s.find(x);  // 返回集合s中等于x的元素的迭代器，若不存在，返回s.end()
s.lower_bound(x)  // 查找大于等于x的最小的那个元素，返回该元素的迭代器
s.upper_bound(x)  // 查找大于x的最小的元素，返回该元素的迭代器
s.erase(x)  // 从s中删除元素x，时间复杂度为O(logn) （可以传入元素x或者指向元素x的迭代器）
```

- `map`

map容器是一个键值对key-value的映射，其内部实现是一棵以key为关键码的红黑树。Map的key和value可以是任意类型，其中key必须定义小于号运算符。

```c++
#include<map>  // 头文件
map<int, int> mp;
map<pair<int, int>, vector<int> > test;

size/empty/clear/begin/end均与set类似。

// 插入和删除
mp.insert(pair<key_type, value_type>)  // 插入键值对
示例：
mp.insert(pair<int, int>(1, 10));
mp.insert(make_pair(2, 20));
mp.insert(map<int, int>::value_type(3, 30));
mp[4] = 40;
mp.insert({1,2});
----------------------------------------------------
mp.erase(1);  // 删除容器中值为key的元素
mp.erase(mp.begin());  // 删除迭代器所指的元素
mp.erase(mp.begin(), mp.end());  // 删除迭代器所指区间的元素

// 查找和统计
mp.find(key);  // 查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回mp.end()
mp.count(key);  // 统计键为key的键值对的个数

// map的遍历
for(map<string, int>::iterator it = mp.begin(); it != mp.end(); it ++)
cout << it->first << "---->" << it->second << endl;
```

- `pair`

成对出现的数据，利用对组可以返回两个数据

```c++
#include<utility>  // 头文件
pair<int, int> p;  // 定义pair
pair<int, int> p(1, 2);  //定义的同时初始化
p = make_pair(2, 3);  // 使用make_pair函数初始化
cout << p.first << " " << p.second << endl;  // 访问pair中的值
```

## 8.位运算和常用库函数

- 位运算

| 位运算符 | &      | \|     | ~      | ^        | >>   | <<   |
| -------- | ------ | ------ | ------ | -------- | ---- | ---- |
| 含义     | 按位与 | 按位或 | 按位非 | 按位异或 | 右移 | 左移 |

```c++
// 位运算常用技巧
(x >> k) & 1  // 求x的二进制中第k位是什么 例如：(8 >> 3) & 1,结果为1
x & -x  // 返回x的最后一位1
x & (x - 1)  // 消去x的二进制中从右往左第一个1，常用来计算一个数的二进制有多少个1
a >> k  // 相当于除以2^k
a << k  // 相当于乘以2^k
a ^ b ^ b = a  //  异或运算的自反性
void swap(int &a, int &b) {  // 用异或运算来交换变量a和b的值
  a ^= b;
  b ^= a;
  a ^= b;
}
```

- 库函数

```c++
#include<algorithm>  // 头文件
(1)reverse函数用于反转在[first,last)范围内的顺序（包括first指向的元素，不包括last指向的元素），reverse函数没有返回值.(左闭右开)

reverse(a.begin(), a.end());  // 翻转一个vector
reverse(a + 1, a + n);  // 翻转一个数组，元素存放在下标1~n

(2)unique函数返回去重之后的尾迭代器（或指针），仍然为前闭后开，即这个迭代器是去重之后末尾元素的下一个位置。该函数常用于离散化，利用迭代器（或指
针）的减法，可计算出去重后的元素个数。注意：传入的序列必须先排好序！！！
int m = unique(a.begin(), a.end()) - a.begin();  // 返回vector中去重后元素的个数
int m = unique(a + 1, a + n + 1) - (a + 1);  // 返回数组中去重后元素的个数，元素存放在下标1~n
a.erase(unique(a.begin(), a.end()), a.end());  //对于vector容器a,去除去重后容器中的重复元素

                  
(3)random_shuffle函数使用默认的随机数生成器(比如c语言中的rand())来打乱[first, last)之间的元素顺序.(左闭右开)
random_shuffle(a.begin(), a.end());  // 打乱vector中a.begin()到a.end()的元素顺序
random_shuffle(a + 1, a + n + 1);  // 打乱数组a中(a + 1)到(a + n + 1)的元素顺序

(4)sort函数对两个迭代器（或指针）指定的部分进行快速排序。可以在第三个参数传入定义大小比较的函数，或者重载“小于号”运算符。                   
sort(a.begin(), a.end());  // 将a.begin(), a.end()之间的元素进行排序，默认为从小到大
sort(a + 1, a + n + 1, cmp);  // 把一个int数组（元素存放在下标1~n）从大到小排序，传入比较函数,实现从大到小排序    
bool cmp(int x, int y)
{
	return x > y;
}

struct Rec  // 对于自定义数据类型，需要重载运算符
{
	int x, y;
	bool operator < (const Rec &t) const
	{
		return x < t.x;
	}
};
vector<Rec> vec;
sort(vec.begin(), vec.end());

(5)lower_bound/upper_bound()
lower_bound 的第三个参数传入一个元素x，在两个迭代器（指针）指定的部分上执行二分查找，返回指向第一个大于等于x的元素的位置的迭代器（指针）。
upper_bound 的用法和lower_bound大致相同，唯一的区别是查找第一个大于x的元素。注意:两个迭代器（指针）指定的部分应该是提前排好序的!!!

在有序int数组（元素存放在下标1~n）中查找大于等于x的最小整数的下标:
int l = lower_bound(a + 1, a + 1 + n, x) – a;

在有序vector<int> 中查找小于等于x的最大整数（假设一定存在）:
int y = *--upper_bound(a.begin(), a.end(), x);

(6)next_permutation()用于得到迭代器范围 [first, last] 的所有可能的排列中的下一个排列(按字典序)，如果存在这样的“下一个排列”，返回true并执行排列，否则返回false。目标是一个可以遍历的集合（如string，如vector或者数组）
int nums[] = {1, 2, 3};
do
{
	cout << nums[0] << " " << nums[1] << " " << nums[2] << endl;
}while(next_permutation(nums, nums + 3));
```

- 用两个栈实现队列:第一个栈用于插入元素，模拟进队操作；第二个栈用于辅助第一个栈来弹出元素，模拟出队操作

```c++
class MyQueue {
public:

    stack<int> st1, st2;
    
    /** Initialize your data structure here. */
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        st1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while(st1.size() > 1)
        {
            st2.push(st1.top());
            st1.pop();
        }
        int t = st1.top();
        st1.pop();
        while(st2.size())
        {
            st1.push(st2.top());
            st2.pop();
        }
        return t;
    }
    
    /** Get the front element. */
    int peek() {
        while(st1.size() > 1)
        {
            st2.push(st1.top());
            st1.pop();
        }
        int t = st1.top();
        while(st2.size())
        {
            st1.push(st2.top());
            st2.pop();
        }
        return t;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        if(!st1.size())
        return true;
        else return false;
    }
};

```
