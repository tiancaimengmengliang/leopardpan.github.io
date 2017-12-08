---
layout: post
title: "泛型与模板"
data: 2017-12-7
description: "泛型与模板入门"
tag: STL
---

## 泛型的基本概念 ##


----------

    int fun(int x, int y)
    {
        return x > y ? x : y;
    }
    double fun(double x, double y)
    {
        return x > y ? x : y;
    }


----------
以上代码实现的函数功能一样，代码除数据类型不一样以外，其余的都一样，为了避免写这种大量的重复代码，并且使得函数适用于更多的数据类型，有了泛型(generic Type)

## 函数模板 ##

----------

    T fun(T x, T y)
    {
        return x > y ? x : y;
    }

----------
T可以看成是抽象出来的类型，通用的类型。但是这样的带并不能直接的使用，真正的程序是将T泛型替换成真正数据类型的代码，这样的含有泛型代码适用数据类型的能力很强，称为模板。


----------


    #include<iostream>
    using namespace std;
    
    template<typename T, typename S>
    T fun(T x, S y)
    {
        return x > y ? x : y;
    }
    int main()
    {
        cout<<fun<int, int>(4,6)<<endl;
        cout<<fun<double, int>(4.56,3.21)<<endl;
        return 0;
    }


----------
template用来申明泛型变量，比方说，当函数传入的变量可能是两个不同的类型时 ，可以申明两个typaname。
函数调用时，需要显示的传入template申明的变量的类型。
由于调用函数时，常常传入的参数类型就是模板所要调用的类型，编译器可以从函数参数类型中推测出模板函数做需要的真实参数，所以可以省略调用语句中的尖括号。


----------

    #include<iostream>
    using namespace std;
    template<typename T, typename S>

    T fun(T x, S y)
    {
        return x > y ? x : y;
    }
    int main()
    {
        cout<<fun(4,6)<<endl;
        cout<<fun(4.56,3.21)<<endl;
        return 0;
    }
    


----------
## 类模板 ##

类模板同函数模板的用法相同，在需要替换数据类型的地方使用泛型，如一下代码


----------

    #include<iostream>
    using namespace std;
    #define PI 3.14

    template<typename T>
    class Circle
    {
        private :
        T Radius;
        public :
        Circle(T r);
        T Area();
    };
    
    //类外定义方法
    template<typename T>
    Circle<T>::Circle(T r)
    {
        Radius = r;
    }
    
    //有返回值的格式
    template<typename T>
    T Circle<T>::Area()
    {
        return PI*Radius*Radius;
    }

    int main()
    {
        Circle<int>circle_1(10);
        cout<<circle_1.Area()<<endl;
        return 0;
    }


----------
模板参数的关键字除了typaname外，还有class，为了避免和类的关键字class混用，一般用typename
