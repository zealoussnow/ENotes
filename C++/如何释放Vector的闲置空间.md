#### 1. std::vector的容量操作

C++ STL容器vector对于容量的操作是只增不减，如下面的代码：

```cpp
	vector<int> v;
	v.push_back(12); // capacity: 1
	v.push_back(22);// capacity: 1 * 2 = 2
	v.push_back(32);// capacity: 2 * 2 = 4
	cout << v.capacity() << endl; // 4
	v.insert(v.begin(), 12, 86);
	cout << v.capacity() << endl; // 15
	// 删除第 3 个到倒数第 2 个元素（不含）之间的所有元素
	v.erase(v.begin() + 2, v.end() ­- 2);
	cout << v.size() << endl; // size: 4
	cout << v.capacity() << endl; // 15 <­­­ #1
	v.clear();
	cout << v.capacity() << endl; // 15 <­­­ #2
	v.reserve(0);
	cout << v.capacity() << endl; // 15 <­­­ #3
```

由上面的代码可以看出，不论是删除vector中的元素(#1)、甚至clear整个容器的内容(#2)、或显式将capacity保留为0(#3)，都无法减小vector中闲置的空间。

#### 2. std::vector复制构造不会复制capacity

如下列代码:

```cpp
	vector<int> v2;
	v2.push_back(12);
	v2.push_back(28);
	cout << v2.capacity() << endl; // 2
	v2.reserve(120);
	cout << v2.capacity() << endl; // 120
	cout << v2.size() << endl; // 2
	vector<int> v3(v2);
	cout << v3.capacity() << endl; // 2  <­­ #1
	cout << v3.size() << endl; // 2
```

如上所示，v3的capacity只是2(#2)，即v2中的元素个数。

#### 3. 通过复制构造和swap来释放vector容器闲置的内存空间

如下列代码：

```cpp
	vector<int> v2;
	v2.push_back(12);
	v2.push_back(28);
	cout << v2.capacity() << endl; // 2
	v2.reserve(120);
	cout << v2.capacity() << endl; // 120
	cout << v2.size() << endl; // 2
	vector<int> (v2).swap(v2); // <­­ #1
	cout << v2.capacity() << endl; // 2  <­­ #2
```

<font color="green">为什么可以减小v2的capacity？</font>

* vector<int>(v2)调用vector的复制构造函数，用v2的元素来构造一个新的、临时对象(无名对象)

* 由于是复制构造，所以新的、临时对象的capacity是v2的元素的个数，所以为2

* 由于成员函数swap()交换两个容器的一切：包括所有迭代器、size、所有元素甚至capacity

* 经过 swap()后，v2的capacity变成新的、临时对象的capacity，即2，对应的：临时对象的capacity变成120

* 由于vector<int>(v2)创建的临时对象在vector<int>(v2).swap(v2);这个语句结束后销毁，至此v2的capacity为2，原先闲置的空间(120-2个元素的空间)被释放(随着临时容器对象的销毁而释放)。

同理，完全清除一个vector的所有存储：

```cpp
	vector<int> v2;
	v2.push_back(12);
	v2.push_back(28);
	cout << v2.capacity() << endl; // 2
	v2.reserve(120);
	cout << v2.capacity() << endl; // 120
	cout << v2.size() << endl; // 2
	vector<int> ().swap(v2); // <­­ #1
	cout << v2.capacity() << endl; // <­­ #2
```

* (#1)首先创建一个临时(空) 容器，然后与v2进行swap()

参考链接：
[如何释放 Vector 容器闲置的空间](http://www.xuanyuansoft.com/images/files/Reference%20-%20How%20to%20Release%20Vector's%20Storage.pdf)

---------------------------------------

#### vector的insert和push_back的区别

std::vector::insert(在容器的指定位置插入元素)

* 如果新的size()比旧的capacity()大，会导致重新分配空间。如果新的size()比capacity()大，所有的迭代器和引用是无效的。另外，只有在插入位置之前保留的迭代器和引用是有效的，超过结尾的迭代器同样是无效的。

std::vector::push_back(将给定的元素添加到容器的末尾)

* 新的元素被给定元素的一份拷贝所初始化

* 给定的值移动到新的元素当中

* 如果新的size()比capacity()大，所有的迭代器和引用(包括超过结尾的迭代器)是无效的，否则只有超过结尾的迭代器是无效的。

参考链接：
[C++ vector's insert & push_back difference](http://stackoverflow.com/questions/13324431/c-vectors-insert-push-back-difference)