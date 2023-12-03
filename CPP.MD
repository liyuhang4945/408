# cpp 20 使用

添加HeadUnits.h

```
#pragma once
import<iostream>;
import<vector>;
import<utility>;、、等等
```

# CPP核心编程

## 内存分区模型

- 代码区：存放代码的二进制代码 操作系统管理
  - 共享。只读
- 全局区：存放全局变量和静态变量以及常量
  - 常量区，字符串常量 其他
  - 由操作系统释放
    - 局部
      - 局部变量
      - const修饰的局部常量
    - 全区
      - 全局变量
      - 静态变量
      - 常量
      - const 修饰的全局常量 字符串常量
- 栈区：局部变量
  - 注意
    - 不返回局部变量的地址
- 堆区：程序员分配和释放



### 意义

​	赋予不同的生命周期，更灵活的编程



## 引用：给变量一个别名

- ​	数据类型&别名=原名
- 引用的传参效果等同于传址
- 做函数的返回值
  - 不用返回局部变量的引用
  - 函数的调用可以作为左值
- 本质
  - 一个指针常量 指针不可改，指向的量可以改
- 常量引用
  - 修饰形参，防止误操作
  - 打印函数

## 函数高级

1. 默认参数
   1. 默认参数位于最后
   2. 声明有，实现不能有、二意性
2. 占位参数
   1. void func(int a,int)
   2. 可以有默认参数
      1. void int a,int = 10
3. 函数重载
   1. 同一个作用域
   2. 名称相同
   3. 参数类型不同或个数不同 或顺序不同
   4. 引用作为重载的条件
      1. void func（int& a） = 10 //10在栈区 不合法的内存空间	
      2. void func（const int& a） = 10  合法 语法优化，临时给了一个空间
   5. 重载时避免默认参数



## 类与对象

### 封装

- ​	将属下和行为作为主题，表现生活中的事务
- 将属性和行为加以权限控制
- 三种访问权限
  - public	
  - protected
  - private

struct和class 的区别

- struct 默认权限是 public
- class 默认 private

成员属性私有化的好处

- 

### 初始化与清理

#### 构造函数与析构函数

#### 分类与调用

​	不要用拷贝构造函数，初始化匿名对象，编译器会认为是对象的声明



隐式

Person p = 10； === Person p=Person（10）

Person p2 = p  拷贝构造



##### 拷贝构造函数的调用时机

1. 使用一个已经创建的对象初始化一个新东西
2. 值传递的方式给函数参数传值
   1. 会创建一个新对象 调用拷贝构造函数
3. 值方式返回局部对象



#### 构造函数调用规则

c++编译器会给每个类添加桑函数

1. 默认构造
2. 默认析构
3. 默认拷贝

#### *深拷贝与浅拷贝

1. 浅拷贝：简单的赋值拷贝操作
   1. 带来的问题，堆区内存重复释放
2. 深拷贝：重新申请的空间，进行的拷贝操作

#### 初始化列表

```cpp
class Person{
    public:
    Person(int a,int b):属性(a),
    {}
}
```

- 初始化列表和传统初始化操作的区别？



#### 静态成员

- 静态成员变量
  - 所有对象共享数据
  - 编译阶段分配内存
  - 类内声明
  - 类外初始化
- 静态成员函数
  - 所有对象共享同一个函数
  - 静态对象共享同一个
  - 只能访问静态成员变量

### 对象模型与this指针

### 成员变量与成员函数分开存储

- 空对象站一个字节，为了区别空对象在内存中的位置
- 非静态成员变量和函数都不属于类对象上。

#### this指针

- 指向被调用的成员函数所属的对象
- 用途
  - 形参与成员变量同名时，this区分
  - 类的非静态成员函数中返回对象本身，return *this
- 使用时要进行判空

### 空指针调用成员函数

- 添加判空

### const修饰成员函数

- 常函数
  - 内部不可修改成员属性
  - 成员属性声明时添加mutable 可以修改
  - 修饰的时this指针
  - 让指针指向的值不可修改
- 常对象
  - 只能调用常函数
  - 声明对象前+const

### 友元

##### 友元让一个函数或类访问另一个类中私有函数 friend

##### 全局函数

friend void func();

##### 类做友元

friend class Person;

##### 成员函数友元

friend void GoodGay::visit();



### 运算符重载

也可以发生函数重载

##### 加号

```cpp
Person operator+(Person& p) {
		Person temp{ 0 };
		temp.age = this->age + p.age;
		return temp;}
//成员函数

//全局函数重载
Person operator+(Person& p1, Person& p2) {
	Person temp;
	temp.age = p1.age + p2.age;
	return temp;
}
```



##### 左移运算符：只能全局函数，应为无法实现cout在左边

```cpp
ostream& operator<<(ostream& cout, Person& p) {
	cout << p.age;
	return cout;
}
osream对象 只能有一个
友元实现输出私有内容
```

##### 递增运算符 实现自己的整形数据

```cpp
//前置
myInteger& operator++（）
{
    m.Num++;
    return * this;
}
//占位参数区分前置后置
myInteger& operator++（int）
{
    //
    myinteger temp = *this;
    m.Num++;
    return temp;
}
```

##### 赋值运算符

- c++给每一个类添加四个函数

  - 默认构造
  - 析构
  - 拷贝
  - 赋值 同样有深浅拷贝问题

- ```cpp
  pERSON operator=(Person& p)
  {
      浅拷贝
  //    m.age=p.age;
      
      //深拷贝
      if（m.age!=NULL）{
          delete m.age;
      }
      m.age=new int(*p.age);
      RETURN *THIS;
  }
  ```

  

##### 关系运算符

```
bool operator (Person& p)
if this
	return true;
return false;
 
```

##### 函数调用运算符重载

仿函数

```cpp
 void operator()(string text){
	cout<<text<<endl;
}
、、匿名函数对象
MyAdd()(100,100)
```

### 继承

- class 子类 ： 继承方式 父类

#### 继承方式

无法访问父类的私有属性

- 公共
  - public 和protected 无变化
- 保护
  -  public 和protected 都变为protected
- 私有
  - 都变为私有的

c1/d1 reportSingleClassLayout(类名) 文件名

- 通过子类访问父类同名属性或者函数，需要添加作用域
- son.base::属性
- 子类同名会隐藏掉父类中的成员函数



#### 同名静态成员的处理方式

- 和非静态一样

#### 多继承

class 子类：方式 父类，方式 父类。。。。



#### 菱形继承的问题

```cpp
两个派生类继承自同一个基类
莫格类继承这两个派生类
				动物
				
	羊							骆驼
	
				羊驼
```

1. 问题1

   1. 两个父类拥有相同的数据，
      1. 解决方法 ，加以作用域区分

2. 问题2  

   1. 菱形继承导致 多份数据，造成资源浪费？

      1. 解决方法，虚继承，解决菱形继承

      2. 继承前加上关键字 virtual 变为虚继承 

         ```
         class Sheep:virtual public Animal{};
         ```

      3. Animal 为虚基类

         1. VBptr 虚基类指针 指向 vbtable   （virtual base table）





### 多态

两类

- 静态多态 和动态多态
- 静态多态
  - 早绑定，编译阶段确定函数地址
- 动态多态
  - 运行阶段确定函数地址
  - 需要满足的条件，
    - 有继承关系，
    - 父类 方法前有virtual 关键字
    - 子类重写父类中的虚函数
  - 使用，
    - 父类的指针或引用，执行子类对象

#### 基本概念

1. 父类指针指向子类



#### 原理

- vfptr -》 vftable 虚函数表指针
- 表内记录虚函数的地址
- 子类重写父类的虚函数
  - 子类中的虚函数表内部会替换成子类的虚函数表



#### 纯虚函数和抽象类

- ```
  virtual 返回值类型 函数名 （参数列表）=0；
  ```

- 有一个纯虚函数就是抽象类

- 抽象类的特点

  1. 无法实例化对象
  2. 子类必须重写父类的纯虚函数

#### 虚析构和纯虚析构

- 问题，子类继承中开辟堆属性，释放时无法释放
- 父类指针 析构时，不会调用子类中析构的方法
  - 解决方法
    - 将父类的析构方法改为虚析构 前添加virtual
  - 纯虚析构
    - 需要声明和实现

## 文件操作

<fstream> 头文件

三类

- ​	ofstream 写
- ifstream 读
- fstream  读写



文件类型

- 文本文件 ascii 码
- 二进制文件

### 文本文件

```
1 #include<fstream>

of stream ofs;
ofs.open("路径",打开方式)；
写入数据
ofs《《“data”；
ofs.close();
```

打开方式

1. ​	ios：：in  只读
2.  ios：： out  只写
3.  ：：ate  初始位置：文件尾
4. app：追加方式写
5. trunc    如果存在，先删在创建
6. binary  二进制方式

 配合使用  利用 | 操作符

```
ifstream ifs;
	ifs.open("text.txt", ios::in);
	if (!ifs.is_open())
	{
		cout << "失败" << endl;
		return;
	}
	//第一种
	/*char buf[1024] = { 0 };
	while (ifs >> buf)
	{
		cout << buf << endl;
	}*/

	char buf[1024] = { 0 };
	//第二种
	/*while (ifs.getline(buf, sizeof(buf))
	{
		cout << buf << endl;
	}*/
3
	string s;
	while (getline(ifs, s))
	{
		cout << s << endl;
	}

	ifs.close();
```

### 二机制操作

#### 写入

```
class Person
{
	char Name[64];
	int age;
}；

ofstream ofs;
ofs.open("person.txt",ios::out | ios::binary);
Person o ={};
ofs.write((const char *)&p,sizeof(Person));

ofs.close();
```

#### 读 

istream 

```
ifstream ifs；
ifs.open("",ios::in | ios:: binary);
if(!ifs.is_open())
{
	return;
}
Person p;
ifs.read((char *)&p,sizeof(Person));

ifs.close();
```

# stl

## vector

和数组相似

单端数组

vector 可以动态扩展

- 动态扩展
  - new 一个更大的内存空间，将元数据拷贝过去，释放原空间

### 赋值

```
vector<> v;
vector<> v(v.begin.v.end);
vector<> v3(n, m)?//n 个m值
v4 = v1;
v.assign(n,m)

v.assign(v.begin,v.end)
```

### 容量和大小

```
empty
capacity
size
resize（num）
resize（int，elem）
```

### 插入和删除

```
push_back
pop_back
insert(const_iterator,pos，ele) 指定位置插入
insert(const_iterator,pos，count,ele)指定位置插入n个 
erase(it,pos) 删除指定位置
erase(it.start,it.end) 删除指定位置从 start删到end
clear
```

### 存取

```
at（index）
front
back
```

### swap收缩内存空间

vector<>(v).swap(v);

vector<>(v) 匿名对象

### 预留空间

reserve（len） 预留位置不可访问

- 减少vector动态扩展容量时的扩展次数

## deque

- 双端数组，头端进行插入删除

内部工作原理

- 内部中控器，维护每段缓冲区的内容，缓冲区中存放真实数据
- 中控器维护每个区的地址
- 迭代器支持随机访问

### 构造函数

- deque<T> deqT;
- deque(beg,end)
- deque(n,elem)
- deque(deqT);

### 赋值

- ```
  deque<int>d2;
  	d2 = d1;
  	printDeque(d2);、、
  	deque<int>d3;
  	d3.assign(d1.begin(), d1.end());、、
  	printDeque(d3);
  	deque<int>d4;
  	d4.assign(10, 100);、、
  	printDeque(d4);
  ```

### 大小

- resize(size);
- resize(size,elem);
- resize(5);
- empty();
- size();

### 插入与删除

- push_back

- push_front

- pop_back

- pop_front

- insert(d1.begin(),d2.begin(),d2.end()); 100 100 ---->1 2 3  1000  

- erase(d1.begin());

- ```
  deque<int>::iterator it = d1.begin();
  it++;
  de.erase(it);
  删除指定位置
  ```

- d1.erase(d1.begin,d1.end)

- clear

- 提供的位置是迭代器

### 存取数据

- at（index）
- operator[] 索引指向的数据
- front
- back

### 排序

- sort（iterator beg，iterator end）d.begin,d.end 从小到大
- 

## stack容器

**概念**：先进后出 顶端可用



### 构造

- stack<T>
- stack(const stack &stk)

### 赋值

- stack& operator= (const stack& stk)

### 存取

- push
- pop
- top

### 大小

- empty
- size

## queue

- 概念  先进先出  不可遍历

### 构造

- queue<T>que
- queue  (const queue& que)

### 赋值

- operator(const queue &que)

### 数据存取

- push
- pop
- back
- front

### 大小

- empty
- size



## LIST

功能：链式存储

链表由一系列结点组成

- 优点
  - 快速插入
  - 动态内存分布 
  - 双向迭代器
  - 插入删除不会使以生成的迭代器失效
- 缺点
  - 占用空间大
- 双向循环链表

### 构造

- list<T> list
- list (const list& list)
- list(beg,end)
- list(n,elem)

### 赋值

- assign(beg,end)
- assign(n,elem)
- swap(list)
- list&operator= (const list &list)



### 大小

- size
- empty
- resize（num）
- resize（n，elem）

### 插入与删除

- push back
- pop back
- push_front
- pop_front
- insert(pos,elem)
- insert(pos,n,elem)
- insert(pos,beg,end)
- clear
- erase(beg,end)
- erase(pos)
- remove(elem)

### 存取

- front
- back

### 反转与排序

- reverse
- L.sort
  - 内部排序算法
  - L1.sort（myCompare）

## set/multiset容器

- 所有元素在插入时自动被排序
- 本质
  - 关联式容器		二叉树实现
- 区别
  - set不允许元素重复
  - multiset 允许

### 构造和赋值

- set<T> st
- set<T> st(const set& st)
-  set  = set

### 插入与删除

- insert
- clear
- erase(pos) 迭代器
- erase(beg，end)
- erase（num）



### 大小与交换

- size
- empty
- swap

### 查找与统计

- find（key）  返回一个迭代器  true  ？ 迭代器： set。end
- count（key）  统计key的元素个数



### set和multiset的区别

- pair<set<int>::iterator,bool> ret=s.insert(num);
  - if(ret.second)  cout<<"success"<<endl;
- multiset 不做检测 

### pair对组创建

- pair<string,int> p{"Tom",20} 
  - cout<<p.first<<p.second
- pair<string,int> p = make_pair("tom",20);

### set容器排序

- 利用仿函数改变排序规则

- ```
  class Mycompare
  {
      public:
          bool operator()(int v1,int v2)
          {
              return v1> v2
          }
  }
  ```

- set<int,MyCompare> s2;

- set<Person,comparePerson>

  - ```
    bool operator()(const Person& p1,const Person& p2)
    {
    return p1.age > p2.age;
    }
    ```

    

## map/multimap

- map中所有元素都是 pair
- pair <key,value> 
- 所有元素根据key值自动排序
- 关联式容器  底层二叉树
- 根据key快速找到value



### 构造

- map<T1,T2> map
- map(const map& map)
- operator= 

- m.insert(pair<int,int>(1,10));



### 大小和交换

- size
- empty
- swap（st）

### 插入和删除

- insert（make_pair(key,value);
- insert(map<int,int>::value_type(key,value));
- m[key]=value;
- erase(m.begin)
- erase(key)

### 查找与统计

- it = find();\

### 排序

- 仿函数改变排序规则

- ```
  MyCompare{
  public:
  	bool operator()(int v1,int v2)
  	{
  		return v1> v2
  	}
  }
  
  map<int,int.myCompare>
  ```

  



## 函数对象

### 

- 重载函数调用操作符的类，称为函数对象

- 特点

  - 想普通函数那样调用
  - 有自己的状态
  - 可以作为参数传递

- ```
  class MyAdd
  {
  public:
  	int operator()(int v1,int v2)
  	{
  		return v1+v2;
  	}
  }
  
  int main()
  {
  	MyAdd ma;
  	cout<< ma(10,20)<<endl;
  }
  ```

## 谓词

- 概念

  - 返回bool类型的仿函数称为谓词
  - 一个参数时一元谓词
  - 两个是二元谓词

- ```
  class MyAdd
  {
  public:
  	bool operator()(int val)
  	{
  		return val > 5;
  	}
  }
  
  
  
  find(v.beg,v.end,greateFive());匿名函数对象
  
  sort(v1.beg,v1.end,myCompare()));
  ```

  

## 内建函数对象

- #include<functional>
- 算术
  - negate<int> n;  取反
    - n(50)=-50;
  - plus<int> p;
    - p(10,20)=30;
- 关系
  - equal_to
  - not_equal_to
  - greater<T>
    - sort(v.begin,v.end, greater<int>());改变排序规则
  - less<T>
- 逻辑
  - and
  - or
  - not
  - 使用场景
    - vector<bool>b2;
    - v2.resize(v.size());
    - transform(v.begin,v.end,v2.begin,v2.end,logical_not<bool>());将v1的容器搬到v2的容器中并取反

## 常用算法

- <algorithm> <functional> <numeric>

### 常用遍历算法

- for_each 

- transform

- ```
  for_each(v.begin,v.end(),function)		
  printval(int val);每一次都调用这个，传入it
  仿函数print()
  ```

### 常用查找

- find

  - beg,end,value

  - 找到返回位置迭代器，找不到返回末尾位置迭代器 

  - 自定义数据类型需要重载==符号 

    - ```
      bool operator==(const Person&p){
      	if(this.xx==p.xx)
      	{
      		return true;
      	}else
      	{
      		return false;
      	}
      }
      ```

      

- find_if

  - beg,end,pred(bool类型的仿函数)

  - ```
    find_if(beg,end,function)
    ```

- adjacent_find

  - ```
    返回相邻重复元素的的第一个的位置
    ```

    

- binary_search

  - 无序序列中不可用

- count

  - (v.beg,v.end,elem)
  - (v.beg,v.end,elem) elem 重载==号

- count_if

  - (v.beg,v.end,elem，prep)

### 排序

- sort（beg,end）
- sort（beg,end,greater<T>）
- random_shuffle(beg,end)、、打乱排序
- merge
  - 两个有序容器元素合并，存储到另一个容器
  - des.resize();
  - merge（beg,end,beg2,end2,des)
- reverse
- (v.beg,v.end);



### 拷贝

- copy
  - (beg,end,target.beg)
- replace
  - (beg,end,oldValue,newValue)
- replace_if
  - (beg,end,oldValue,prep,newValue)
- swap



### 算术生成算法

《numeric》

- accumulate、计算容器元素累计总和
  - （beg,end,起始参数）
- fill
  - beg,end,value

### set算法

- set_intersection
  - 两个容器交集的部分，放到一个新容器中
  - itEnd=（v1.beg,v1.end,v2,beg,v2.end,target.beg)
  - vtarget.beg,itEnd,myPrint)
- 并集
  - set_union
  - 有序序列
- 差集
  - set_difference