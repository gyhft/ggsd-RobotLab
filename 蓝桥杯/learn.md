###### 1.C++的引用
C++提供了引用，在功能上比指针弱，但是减少了出错的可能，提高了代码的可读性。使用引用交换两个变量的方法如下：
```C++
#include <iostream>
using namespace std;
void swap2(int& a, int& b){
    int t = a;
    a = b;
    b = t;
}
int main()
{
    int a =3, b =4;
    swap2(a,b);
    cout << a << " " << b << "\n";
    return 0;
}
```
如果在参数名之间加一个“&”符号，就表示这个参数按照传引用的方式传递。这样，在函数内改变参数的值，也会修改到函数的实参。

###### 2.字符串


