1. 临时对象的产生与利用：刻意使用临时对象的方法是，在型别名称之后直接加一对小括号，如int(8)  
2. 静态常量整数在class内部直接初始化：可用通过双冒号直接引用一个类的静态变量  
3. increment/decrement/dereference操作符：即前置加、后置加、前置减、后置减以及返回一个变量的地址值的具体源码实现  
4. 前闭后开区间表示法：以[fisrt,last)来表示，整个实际范围从first开始，到last-1结束，last迭代器指的是最后一个元素的下一个位置。由`find`和`for_each`的源码来展示
5. functioin call操作符（operator()）：通过重载operator()实现仿函数（将一个具有一个模板的结构体变成了一个仿函数，我认为叫它仿函数的精髓也在这里吧，因为本来它定义的是一个结构体而不是一个函数，但是通过重载了operator()让它获得了函数的功能——可以处理参数并且具有返回值），如：  
```cpp
template <class T>
struct plus {
	//重载operator操作符
	T operator() {const T& x,const T& y} const {return x + y;}
};

int main() {
	//产生仿函数对象
	plus<int> plusobj;
	//调用仿函数
	cout << plusobj(3,5) << endl; //8
}
```