### 1.变量的声明和定义有什么区别

为变量分配地址和存储空间的称为定义，不分配地址的称为声明。

一个变量可以在多个==地方==声明，但只能在一个地方定义

加入extern修饰的是变量的声明，说明此变量将在文件以外或者文件后面部分定义

```C++
// File: main.cpp

// 第一次声明
extern int myVariable;

int main() {
    // 在局部作用域中的定义
    int myVariable = 42;

    // Other code using the local myVariable

    return 0;
}

// File: other_file.cpp

// 第二次声明
extern int myVariable;

// 在全局作用域中的定义
int myVariable = 0;

```

### 2.写出bool,int,float,指针变量与“零值”比较的if语句？

```C++
//bool
if(flag){A;}
else {B;}
//int
if(0!=flag){A;}
else {B;}
//指针
if(NULL==flag){A;}
else {B;}
//float

```

如果由于浮点数表示的特性，`f` 的值非常接近零但不是精确的零，你可能需要使用一个小的 epsilon 值来考虑可能的舍入误差。例如：

```cpp
const float epsilon = 0.0001;

if (f >= -epsilon && f <= epsilon) {
    std::cout << "乐";  // 在控制台打印 "乐"
}
```

这允许在零周围有一个小的容差（`epsilon`），将在该范围内的值视为实际上等于零。根据你的具体需求和精度考虑，调整 `epsilon` 的值。

浮点数有限精度的表现，对于一些无理数，会有舍入的操作

### 3.sizeof和strlen()的区别

- sizeof是一个操作符，可以计算变量和数据类型的大小，在编译时就计算出来了，并且计算的是数据类型占内存的大小
- strlen是库函数，只能计算结尾带有‘\0’终结符的字符串，计算的是字符串的实际大小（不包括\0)

### 4.C和CPP在static的区别

static变量在C中用来修饰局部静态变量和外部静态变量、函数

C++则还能用来定义类的成员变量和函数，即静态成员和静态成员变量

可以在多个对象实例间进行通信，传递信息

### 5.C中malloc和C++中new的区别

1. new和delete是操作符，可以进行重载，只能在C++中使用
2. malloc和free是函数，可以覆盖，C和C++都可用
3. new可以构造对象，delete可以析构对象
4. malloc和free不能进行对象的构建，仅仅是分配和释放内存
5. new和delete返回的是某种数据类型指针，malloc和free返回void指针

两者不能混用，实现机理不同

### 6.写一个“标准”宏MIN

![image-20231211193050941](../hexocode/source/_posts/EffectiveC/image-20231211193050941.png)

```C++
#define Min(a,b) ((a)<=(b)?(a):(b))
//宏的副作用
Min(++*p,0)//这里因为调用了三元条件运算符，所以导致p加了两次
    
#include<bits/stdc++.h>
using namespace std;
#define DG(sum) if((sum)>0)\
{\
	cout<<(sum)<<" ";	\
}
int main() 
{
	DG(10);	
    return 0;
}
```

### 7.一个指针可以是volatile吗？

[ˈvɒlətaɪl]

可以，因为指针和普通变量一样，有时也有变化程序的不可控性

子中断服务子程序修改一个指向buff的指针时，必须使用volatile修饰这个指针

`volatile` 是一个关键字，用于告诉编译器，所修饰的变量可能会在没有明显的原因下被程序以外的代码更改。这样一来，编译器在优化代码时就不会对这些变量的访问进行优化，以避免潜在的错误。

### 8.a和&a的区别

	int a[5]={1,2,3,4,5};
	int *p=(int*)(&a+1);
	cout<<*(a+1)<<*(p-1);

数组名a指的是数组的首地址

&a指的是数组的指针

### 9.简述C和C++编译的内存分配情况

![image-20231211200045534](../hexocode/source/_posts/EffectiveC/image-20231211200045534.png)

### 10.简述strcpy，sprintf与memcpy的区别

1. 操作对象不同，strcpy的两个操作对象都是字符串，sprintf的操作对象可以是多种数据类型，memcpy的两个对象是任意的内存地址
2. 执行效率 memcpy>strcpy>sprintf
3. 实现功能不同，strcpy主要是实现字符串的拷贝，sprintf主要是实现其他数据类型到字符串的转化，memcpy主要是内存块间的拷贝

![image-20231211200842280](../hexocode/source/_posts/EffectiveC/image-20231211200842280.png)

![image-20231211200847863](../hexocode/source/_posts/EffectiveC/image-20231211200847863.png)

```C++
#include<bits/stdc++.h>
using namespace std;
int main() 
{
	const char*s1="abcdefghijkl";
	char s2[13];
	strcpy(s2,s1);
	cout<<s2<<endl;
	char formattedString[20];  // 假设足够大以容纳结果
    sprintf(formattedString, "%s", s2);
    cout << formattedString << endl;
    char s3[13];
    memcpy(&s3,&s2,sizeof(s2));
    cout<<s3<<endl;
    return 0;
}
```

### 12.面向对象三大特性

封装:

- 将客观事物抽象成类，每个类对自身数据和方法实现不同权限的保护

继承:

- 关于的继承有三种
  - 实现继承（使用基类的属性和方法而无需格外的编码能力)
  - 可视继承（子窗体使用父窗体的外观和方法）
  - 接口继承（仅使用属性和方法，实现滞后到子类实现）

多态:

- 是将父类对象设置成为和一个或更多它的子对象相等的技术。用子类对象给父类对象赋值之后，父类对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。 

- 用一个名字定义多个函数，这些函数执行不同但相似的工作。最简单的多态性的实现方式就是函数重载和模板，这两种属于静态多态性。还有一种是动态多态性，其实现方式就是继承虚函数重写。

- 如果没有虚析构函数，父类指针delete对象就只会调用父类的[析构函数](https://so.csdn.net/so/search?q=析构函数&spm=1001.2101.3001.7020)，如果加上虚析构函数的话，那么，析构父类指针指向的子类对象时候会先调用子类的析构函数，再调用父类的构造函数。

- 如果严格遵守多态的构成条件，那么子类析构函数就算定义成虚函数也无法完成重写了，因为父子类的析构函数是不可能同名的。

  编译器做了一件事，凡是父子类的析构函数，都将父子类的析构函数名变成`destructor()`，其目的就是为了父子类的析构函数可以构成多态。

- 多态的本质就是虚表和虚表指针，虚表本质上是一个数组，存放着所有虚函数的指针。如果父类的虚函数没有被子类改写，那么子类虚函数表的指针就是父类对应的虚函数的指针；否则，虚表的指针是子类虚函数的指针。这个过程在程序运行过程中执行，被称为“动态绑定”；

- 虚函数和纯虚函数两者的区别在于纯函数尚未被实现，定义纯虚函数是为了实现一个接口。在基类中定义纯虚函数的方法是在函数原型后加=0

  > virtual void function() = 0

- 含有虚函数的类叫做抽象类，设计抽象类的目的，是为了给其他类提供一个可以继承的适当的基类。抽象类不能被用于实例化对象，它只能作为接口使用。如果试图实例化一个抽象类的对象，会导致编译错误。

- 万能引用：

  - template <typename T>
    void function(T&& t) {
        otherdef(t);
    }

    引用折叠：当实参为左值或者左值引用（A&）时，函数模板中 T&& 将转变为 A&（A& && = A&）；当实参为右值或者右值引用（A&&）时，函数模板中 T&& 将转变为 A&&（A&& && = A&&）

  - 完美转发：无论调用 function() 函数模板时传递给参数 t 的是左值还是右值，对于函数内部的参数 t 来说，它有自己的名称，也可以获取它的存储地址，因此它永远都是左值，也就是说，传递给 otherdef() 函数的参数 t 永远都是左值。

  - `std::move` 将对象 `t` 转换为右值引用，允许移动语义发挥作用。因此，`forward<Temp>(move(t))` 将 `t` 转换为右值引用，并使用 `forward` 进一步进行转发，调用适当的构造函数，这是合适的。

    Temp y=forward<Temp>(move(t));

  - `std::move` 是一个用于将对象标记为右值引用的函数，它告诉编译器你希望使用移动语义，而不是拷贝语义。

- 那么既然基类的析构函数如此有必要被定义成虚函数，为何类的默认析构函数却是非虚函数呢？
  原来是因为，虚函数不同于普通成员函数，当类中有虚成员函数时，类会自动进行一些额外工作。这些额外的工作包括生成虚函数表和虚表指针，虚表指针指向虚函数表。每个类都有自己的虚函数表，虚函数表的作用就是保存本类中虚函数的地址，我们可以把虚函数表形象地看成一个数组，这个数组的每个元素存放的就是各个虚函数的地址。这样一来，就会占用额外的内存，当们定义的类不被其他类继承时，这种内存开销无疑是浪费的。

==什么是多态？==

多态的基本概念是，同一行为，通过不同的子类，可以体现出来的不同的形态。最简单的多态就是重载和模版，这是静态多态性，动态多态性的实现就是继承虚函数的重写

多态是设置父类对象对应的一个或多个子类对象相等的技术，父类创建一个子类对象，并能够根据子类对象的特性以不同的方式运行。

简单来说，就是一个名字定义多个函数，这些函数执行不同但相似的工作。

动态多态的本质是虚函数表和虚函数指针，每个虚函数继承的类或虚基类都有自己的虚函数表，在运行时实现动态绑定

- ### 内联(inline)函数可以是虚函数吗？

  答：不能，因为内联(inline)函数没有地址，无法把地址放到虚函数表中。

- ### 静态成员可以是虚函数吗？

  答：不能，因为静态成员函数没有this指针，使用类型::成员函数的调用方式无法访问虚函数表，所以静态成员函数无法放进虚函数表。

- ### 构造函数可以是虚函数吗？

  答：不能，因为对象中的虚函数表指针是在构造函数初始化列表阶段才初始化的。

- ###  如何让父类的虚函数无法被重写–final

- ### override

  如何强制要求完成虚函数的重写–override







### 13.C++的空类有哪些成员函数

1. 缺省构造函数
2. 缺省析构函数
3. 缺省赋值运算符
4. 缺省拷贝构造函数
5. 缺省取值运算符
6. 缺省取值运算符 const

只有实际使用这些函数，编译器才会去定义他们

### 14.赋值运算符和拷贝构造函数的区别

![image-20231212111208604](../hexocode/source/_posts/EffectiveC/image-20231212111208604.png)

### 15.设计一个不能继承的类

1. `final` 关键字
2. 将构造函数声明为私有
3. 删除拷贝构造函数和拷贝赋值运算符
4. 使用抽象类和私有构造函数

### 16.访问基类的私有虚函数

下面这方法很难行得通，相当于你收到访问虚函数表，可能就是非法访问内存了

```C++
#include<iostream>
using namespace std;
class A
{
public:
	virtual void g()
	{
		cout << "A::g" << endl;
	}
private:
	virtual void f()
	{
		cout << "A::f" << endl;
	}
};
class B :public A
{
	void g()
	{
		cout << "B::g" << endl;
	}
	virtual void h()
	{
		cout << "B::h" << endl;
	}
};
typedef void(*Fun)(void);
int main()
{
	B b;
	Fun pFun;
	//for (int i = 0; i < 2; ++i)
	cout << &b << endl;
	{
		pFun = (Fun) * ((int*)*(int*)(&b) + 0);
		pFun();
	}
	return 0;
}

```

### 17.简述类成员间重写，重载，隐藏的区别

![image-20231212210645225](../hexocode/source/_posts/EffectiveC/image-20231212210645225.png)

### 18.简述多态实现的原理

编译器发现一个类存在虚函数，会为这个类创建一个虚函数表vtable，虚函数表的各表项指向对应的虚函数指针。

编译器会为这个类中隐含一个vptr指针，指向虚函数表，调用构造函数时，将类与此类的vtable联系起来，指向基础类的指针也变成了指向具体类的this指针，这样就能依靠this指针得到正确的虚函数表

这才能和正确的函数体进行连接，这就是动态联编，实现多态的基本原理。

### 19.链表和数组有什么区别？

1. 存储形式：数组是一块连续的空间，链表是不连续的动态空间
2. 数组查找：数组可以通过索引查找，效率高
3. 数据的插入删除：链表可以快速插入删除节点，数组有可能需要大量的移动
4. 越界问题：数组存在越界问题，链表则不存在

### 20.单链表反序？

```C++
#include<bits/stdc++.h>
using namespace std ;
class ListNode
{
    public:
        ListNode*next;
        int val;
        ListNode(int t):val(t),next(NULL){};
};
ListNode* reverseListNode(ListNode*head,ListNode*t=NULL)
{
    if(head==NULL)return t;
    ListNode*Next=head->next;
    head->next=t;
    t=head;
    reverseListNode(Next,t);
}
int main()
{
    ListNode *head=new ListNode(-1);
    ListNode *temp=head;
    for(int i=0;i<10;++i)
    {
        temp->next=new ListNode(i);
        temp=temp->next;
    }
    temp=head->next;
     while(temp)
    {
        cout<<temp->val<<" ";
        temp=temp->next;
    }
    temp=head->next;
    cout<<endl;
    // ListNode*r=NULL;
    // while(temp)
    // {
    //     ListNode*t=temp->next;
    //     temp->next=r;
    //     r=temp;
    //     temp=t;
    // }
    // head->next=r;
    // temp=head->next;
    head->next=reverseListNode(temp);
    temp=head->next;
    while(temp)
    {
        cout<<temp->val<<" ";
        temp=temp->next;
    }
    system("pause");
    return 0;
}
```

### 21.简述栈和队列的区别

栈和队列都是线性存储结构，但是两者的数据操作不同，栈是先进后出，队列是先进先出，

在stl中，栈和队列都是容器deque的适配器

### 22.使用两个栈实现队列的功能

```C++
#include<bits/stdc++.h>
using namespace std ;
class Stack
{
public:
    Stack(int Val):val(Val),next(NULL){}
    Stack*next;
    int val;
    //入栈
    void Push(int Val)
    {
        Stack*node=new Stack(Val);
        if(this->next==NULL)
        {
            this->next=node;
        }
        else
        {
            node->next=this->next;
            this->next=node;
        }
        return ;
    }
    //出栈
    Stack* Pop()
    {
        if(this->next==NULL)return NULL;
        Stack*temp=this->next;
        this->next=this->next->next;
        return temp;
    }
};

void EnqueueByStack(Stack*S,int data)
{
    Stack*S1=new Stack(-1);
    while(S->next)
    { 
        Stack*temp=S->Pop();
        S1->Push(temp->val);
        delete temp;
    }
    S1->Push(data);
    while(S1->next)
    {
        Stack*temp=S1->Pop();
        S->Push(temp->val);
        delete temp;
    }
}
int main()
{
    Stack *head=new Stack(-1);
    head->Push(1);
    head->Push(2);
    head->Push(3);
    head->Push(4);
    head->Push(5);
    while(head->next)
    {
        Stack*temp=head->Pop();
        cout<<temp->val<<" ";
        delete temp;
    }
    delete head;
    system("pause");
    return 0;
}
```

关注于动态创建和删除内存

### 23.计算一颗二叉树的深度

```C++
#include<bits/stdc++.h>
using namespace std ;
class TreeNode
{
public:
    TreeNode(int Val):val(Val),right(NULL),left(NULL){};
    TreeNode*left;
    TreeNode*right;
    int val; 
};
//递归
int Total_node(TreeNode*root)
{
    if(!root)return 0;
    return 1+max(Total_node(root->left),Total_node(root->right));
}
int main()
{
    TreeNode*root=new TreeNode(1);
    root->left=new TreeNode(2);
    root->left->right=new TreeNode(3);
    root->left->right->right=new TreeNode(5);
    cout<< Total_node(root)<<endl;
    //广度优先也可以查询深度
    system("pause");    
    return 0;
}
```

### 24.编码实现直接插入排序

```C++
#include<bits/stdc++.h>
using namespace std ;
class Solution
{
public:
    void Sort(int *num,int size)
    {
        for(int i=1;i<size;++i)
        {
            int j=i;
            if(num[j]<num[j-1])
            {
                int temp=num[j];
                do
                {
                    num[j]=num[j-1];
                    --j;
                } while (j-1>=0&&temp<num[j-1]);
                num[j]=temp;
            }
        }
    }
};
int main()
{
     int num[10]={0,1,2,5,6,7,8,9,0,10};
     Solution s;
     s.Sort(num,10);
     for(int i=0;i<10;++i)cout<<num[i]<<" ";
    system("pause");    
    return 0;
}
```

### 25.编码实现冒泡排序

 冒泡排序：通过重复遍历待排序的序列，比较相邻的元素，并根据大小交换它们，直到整个序列有序。

```C++
#include<bits/stdc++.h>
using namespace std ;
class Solution
{
public:
    void Sort(int *num,int size)
    {
        
       for(int i=0;i<size;++i)
       {
        bool isChange=false;
        for(int j=size-1;j>i;--j)//相邻元素进行交换，这样每次最后一个必定是最小的
        {
            if(num[j-1]>num[j])
            {
                swap(num[j-1],num[j]);
                isChange=true;
            }
        }
        if(!isChange)return;
       }
    }
};
int main()
{
     int num[10]={0,1,2,5,6,7,8,9,0,10};
     Solution s;
     s.Sort(num,10);
     for(int i=0;i<10;++i)cout<<num[i]<<" ";
    system("pause");    
    return 0;
}
```



### 26.编码实现选择排序

选择排序：每一次从未排序的部分选出最小（或最大）的元素，存放到已排序部分的末尾，直到全部待排序的数据元素排完。

这个和冒泡排序还是很容易弄混的，相当于冒泡排序时交换相邻的，通过这样一步步拿到最小值，而选择排序则是每次选出当前下标最小值然后交换，没有多余的交换

### 27.编码实现堆排序

```C++
#include<bits/stdc++.h>
using namespace std ;
class Solution
{
public:
    void HeapAdjust(int*num,int start,int end)
    {
        int tmp=num[start];
        for(int i=start*2+1;i<=end;i=i*2+1)//每次都需要向下调整所有堆，维持一个个大根堆
        {
            if(i<end&&num[i]<num[i+1])//这里和下面if判断中小于变大于就变成小根堆了
            {
                ++i;//这里使用递增切换被调整的那个堆上有没有需要继续调整的
            }
            if(tmp<num[i])
            {
               num[start]=num[i];
               start=i;//保存个下标，之后直接赋值就行
            }
            else break;
        }
        num[start]=tmp;
    }
    void Sort(int *num,int size)
    {        
       for(int i=(size-2)/2;i>=0;i--)//这一步是得出了最后一个非叶子结点(size-2)/2，递减之后的都是非叶子结点
       {
        HeapAdjust(num,i,size-1);
       }
        for(int i=0;i<size-1;++i)
        {
            swap(num[0],num[size-1-i]);//交换之后再进行堆的调整
            HeapAdjust(num,0,size-1-i-1);
        }
    }
};
int main()
{
     int num[10]={0,1,5,2,9,7,8,6,0,10};
     Solution s;
     s.Sort(num,sizeof(num)/sizeof(num[0]));
     for(int i=0;i<10;++i)cout<<num[i]<<" ";
    system("pause");    
    return 0;
}
```

### 28.编码实现基数排序

```C++
#include<bits/stdc++.h>
using namespace std ;
class Solution
{
public:
    int maxdigit(int *num,int size)
    {
        int d=1;//位数
        int p=10;
        for(int i=0;i<size;++i)
        {
            while(num[i]>=p)
            {
                p*=10;
                d++;
            }
        }
        return d;
    }
    void Sort(int *num,int size)
    {        
        int digits=maxdigit(num,size);
        int k=1;
        list<int>l[10];
        for(int i=0;i<digits;++i)
        {
            for(int j=0;j<size;++j)
            {
                l[(num[j]/k)%10].push_back(num[j]);
            }
            //相当于每轮都筛选出同个10^k里的数，但是因为上一轮排完序，所以下一轮只是对不同位阶的数进行排序而已
            // 1 2 3 11 原始
            // 1 11 2 3 第一轮
            // 1 2 3 11 第二轮
            // 相当于排好了同个位阶的
            for(int j=0,k=0;j<10;++j)
            {
                while(!l[j].empty())
                {
                    num[k++]=l[j].front();
                    l[j].pop_front();
                }
            }
            k*=10;
        }
    }
};
int main()
{
     int num[10]={0,1,4,2,9,7,8,6,0,10};
     Solution s;
     s.Sort(num,sizeof(num)/sizeof(num[0]));
     for(int i=0;i<10;++i)cout<<num[i]<<" ";
    system("pause");    
    return 0;
}
```



### 29.谈谈你对编程规范的理解或认识

- 可行性
- 可读性
- 可移植性
- 可测试性

### 30.short i=0;i=i+1L;这两句有错吗

在数据安全的情况下，大类型的数据向小类型的数据转换一定要显示类型转换

### 31.&&、&、||、|

(1)&&和||判断逻辑关系

(2)| 和 & 对操作数进行求值运算

(3)&& 和 || 都是可以在左侧操作数确定结果的情况下不对右侧操作数进行判断

