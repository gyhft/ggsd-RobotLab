#### 1.结构体

C++中的结构体除了可以拥有成员变量之外，还可以拥有成员函数。下面是一个例子：

```C++
#include <iostream>
using namespace std;

struct Point{
    int x, y;
    Point(int x = 0, int y = 0):x(x),y(y){}
};

Point operator + (const Point& A, const Point& B){
    
    return Point(A.x + B.x, A.y + B.y);
}

ostream& operator << (ostream &out, const Point& p){
    out << "(" << p.x << "," << p.y <<")";
    
    return out;
}

int main()
{
    Point a, b(1,2);
    a.x = 3;
    cout << a + b << "\n";
    

    return 0;
}

```

结构体Point 中定义了一个函数，函数名也叫Point,但是没有返回值。这样的函数称为构造函数（ctor)。构造函数是在声明变量时调用的，例如，声明Point a, b(1,2)时，分别调用了Point() 和Point(1,2)。 Point(int x = 0, int y = 

0):x(x),y(y){} 则是一个简单写法，表示把成员变量x初始化为参数，成员变量y初始化为参数y。也可以写成：

```C++

Point(int x = 0, int y =0){this->x = x, this->y = y;}

```

这里的this是指向当前对象的指针。this->x的意思就是“当前对象的成员变量x",即（*this).x。

接下来为这个结构体定义了”加法“，并且在实现中用到了构造函数。这样，就可以用a+b的形式计算两个结构体a和b的”和“了。

最后，定义这个结构体的流输出方式，然后就可以用cout<< p来输出一个Point结构体p了。

#### 2.排序与检索

例题：大理石在哪里？（Where is the Marble?)

现有N个大理石，每个大理石上写了一个非负整数。首先把各数从小到大排序，然后回答Q个问题。每个问题问是否有一个大理石写着某个整数x,如果是，还要回答哪个大理石上写着x。排序后的大理石从左到右编号为1～N。

样例输入：

4 1 

2 3 5 1

5

5 2

1 3 3 3 1

2 3

样例输出：

CASE# 1:

5 found at 4

CASE# 2:

2 not found 

3 found at 3



```C++
#include <iostream>

#include<algorithm>

using namespace std;

const int maxn = 10000;

int main()
{
    int n, q,x,a[maxn], kase = 0;
    
    while (scanf("%d%d",&n,&q) == 2 && n){
        printf("CASE# %d:\n",++kase);
        for(int i = 0; i < n; i++){
            scanf("%d",&a[i]);
        }
        sort(a,a+n);
        while(q--){
            scanf("%d",&x);
            int p = lower_bound(a,a+n,x) - a;
            if(a[p] === x){
                printf("%d found at %d \n", x , p+1);
                
            }else{
                printf("%d not found\n",x);
            }
        }
    }
    

    return 0;
}
```

sort排序可以用sort(a,a+n)的方式调用，也可以用sort(v.begin(),v.end())的方式调用。lower_bound的作用是查找大于或者等于x的第一个位置。


#### 3.vector

vector就是一个不定长数组。例如，若a是一个 vector,可以用a.size()读取它的大小，a.resize()改变大小，a.push_back()向尾部添加元素，a.pop_back()删除最后一个元素。

vector是一个模板类，所以需要用vector<int> a 或者  vector<double > b 这样的方式来声明一个vector。



#### 4.集合：Set

集合与映射也是两个常用的容器。set就是数学上的集合————每个元素最多只出现一次，和sort一样，自定义类型也可以构造set,但同样必须定义”小于"运算符。


#### 5.映射: Map

map就是从键(key)到值(value)的映射。因为重载了[]运算符，map就像数组的“高级版”。例如，可以用一个map<string,int> month_name来表示“月份名字到月份编号”的映射，然后用month_name["July"] = 7 这样的方式来赋值。

set头文件中的set和map头文件中的map分别是集合与映射。二者都支持insert/find/count和remove操作，并且可以按照从小到大的顺序循环遍历其中元素。map还提供了“[]"运算符，使得map可以像数组一样使用。

#### 6.栈，队列与优先队列

栈，就是符合”后进先出“（Last in First Out, LIFO) 规则的数据结构，有push和pop两种操作，其中push把元素压入栈顶，而pop从栈顶把元素弹出。（top()取栈顶元素但不删除）。

STL的栈定义在头文件<stack>中，可以用stack<int> s方法声明一个栈。

STL队列定义在头文件<queue>中，可以用queue<int> s方式来声明一个队列。

push() 和pop(), front():取队首元素，但不删除。

STL的优先队列也定义在头文件<queue>里，用priority_queue\<int\> pq来声明，由于出队元素并不是最先进队的元素，出队的方法由queue的front()变为了top()。

自定义类型也可以组成优先队列，但必须为每一个元素定义一个优先级。这个优先级并不需要一个确定的数字，只需要能比较大小即可。

可以定义一个结构体cmp,重载"()"运算符，使得其看上去像一个函数。然后用priority_queue\<int, vector\<int\>, cmp\> pq的方式来定义。下面是这个cmp的定义：

```C++

struct cmp{
    bool operator() (const int a , const int b) const {
        
        return a % 10 > b % 10;
        
    }
};

```

对于一些常见的优先队列，如最小队列，可以写成
```C++

priority_queue<int ,vector<int>, greater<int>  > pq

```

