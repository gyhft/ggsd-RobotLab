##### 1.结构体

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



