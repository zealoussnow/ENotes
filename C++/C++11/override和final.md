### 一、override

* 多态行为的基础：基类声明虚函数，派生类声明一个虚函数覆写该虚函数

* 覆盖要求：函数签名一致(函数签名包括：函数名、参数列表、const)

#### 1.派生类希望覆写基类的虚函数，但是虚函数的参数类型不同，如下：

```cpp
	class Base
	{
	public:
		virtual void f(int) { cout << "f in base" << endl; }
	};

	class Derived : public Base
	{
	public:
		virtual void f(short) { cout << "f in derived" << endl; }
	};
```

虽然派生类Derived继承了基类Base并希望覆写函数f，但是在派生类中，f的参数类型与
基类并不相同，此时派生类只是存在另一个同名的函数f。当通过Base类型的指针指向
Derived类型时，调用函数f只会打印出"f in base"。

#### 2.派生类希望覆写基类的虚函数，基类中的虚函数是const的，派生类却不是

```cpp
	class Base
	{
	public:
		virtual void f() const { cout << "f in base" << endl; }
	};

	class Derived : public Base
	{
	public:
		virtual void f() { cout << "f in derived" << endl; }
	};
```

同样，此时基类也没有覆写函数f。

#### 3.overried标识符

```cpp
	class Base
	{
	public:
		virtual void f() const { cout << "f in base" << endl; }
	};

	class Derived : public Base
	{
	public:
		// void f() override // error: 'void Derived::f()' marked 'override', but does not override
		void f() const override { cout << "f in derived" << endl; }
	};
```

在派生类中，使用了标识符overried来明确说明要覆写基类中的虚函数，此时若不加const或者参数
列表与基类不一致，就会在编译时提示错误。

### 二、final

* 让基类不允许被继承

* 希望函数不要再被派生类进一步重写，可以在基类或者派生类中使用final。在派生类中，可以同时
使用overried和final

#### 1.让基类不允许被继承

```cpp
	class Base final { };

	class Derived : public Base { };
```

此时在编译时编译器会提示Derived类无法从Base类继承，因为它已经被声明为final

#### 2.希望函数不要再被派生类进一步重写

```cpp
	class Base
	{
	public:
		virtual void f() final { cout << "f in base" << endl; }
	};

	class Derived : public Base
	{
	public:
		virtual void f() { cout << "f in derived" << endl; }
	};
```

在编译时编译器会提示Base中的f声明为final的函数无法被Derived中的f重写

```cpp
	class Base
	{
	public:
		virtual void f() { cout << "f in base" << endl; }
	};

	class Derived : public Base
	{
	public:
		virtual void f() override final { cout << "f in derived" << endl; }
	};

	class SecondDerived : public Derived
	{
	public:
		virtual void f() override { cout << "f in second derived" << endl; }
	};
```

此时Derived中声明为fianl的函数无法被SecondDerived中的f重写。

#### NOTE

虽然override和final不是C++的关键字，但是在我们用到C++11的这两个特性时，为了防止产生歧义。