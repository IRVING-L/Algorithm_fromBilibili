## 使用C++语言实现[B站王卓老师的数据结构公开课课程代码](https://www.bilibili.com/video/BV1nJ411V7bd?p=1)

| chapter 0        | chapter 1            | chapter 2    | chapter 3            | chapter 4 | chapter 5 |
| :----------------: | :--------------------: | :------------: | :--------------------: | :---------: | :---------: |
| [使用说明](#ch0) | [线性表、链表](#ch1) | [栈、队列]() | [串、数组和广义表]() | [树]()    | [图]()    |





### <span id="ch0">使用说明</span>

**eg.1：**

~~~cpp
//王卓老师视频中常用的结构体定义方式
typedef struct{
    ElemType data[];
    int length;
}SqList; //顺序表类型
~~~

~~~cpp
//本文将使用以下struct定义方式
struct SqList
{
    ElemType data[];
    int length;
};
//或使用class定义一个类
class SqList
{
    ElemType data[];
    int length;
};
~~~

**eg.2：**

~~~c
//C语言中动态分配内存的函数：malloc
malloc(m);//开辟m字节长度的地址空间，返回这段地址的首地址
//王卓老师视频范例
#define MAXSIZE 100
typedef struct{
    ElemType *data;
    int length;
}SqList;
//...//
SqList L;
L.data = (ElemType*)malloc(sizeof(ElemType)*MAXSIZE);
~~~

~~~cpp
//本文将使用以下C++的动态内存分配方式：new
//使用new重写上面的malloc
L.data = new ElemType[MAXSIZE];
~~~



### <span id="ch1">线性表、链表</span>

**==1. 顺序线性表==**

*线性表的定义*

~~~cpp
struct SqList
{
    ElemType *elem;//顺序线性表的表头
    int length;//顺序线性表的长度
};
~~~

*线性表的初始化*

~~~cpp
bool InitList(SqList& L)
{
    L.elem = new ElemType[MAXSIZE]; //在堆区开辟内存
    if(!L.elem)
    {
        cerr<<"error"<<endl;
        return false;
    }
    L.length = 0;//设定线性表长度为0
    return 1;
}
~~~

*线性表的销毁*

~~~cpp
void DestroyList(SqList& L)
{
    if(L.elem)
    {
        delete L.elem;
    }
}
~~~

*线性表的清空*

~~~cpp
void CLearList(SqList& L)
{
    L.length = 0;
}
~~~

*判断线性表是否为空*

~~~cpp
bool IsEmpty(const SqList& L)
{
    return static_cast<bool>(L.length);
}
~~~

***线性表的取值***

~~~cpp
bool GetELem(const SqList& L, const size_t &i, ElemType &e)
{
    if(i<1 || i>MAXSIZE)
    {
        cerr<<"out of range"<<endl;
        return false;
    }
    e = L.elem[i-1];
    return true;
}
~~~

***线性表的查找***

~~~cpp
int LocateList(const SqList& L, const ElemType &e)
{
    for(int i = 0; i<L.length; ++i)
    {
        if(L.elem[i] == e)
        {
            return i+1; //查找成功，返回其查找元素的第一个下标值
        }
    }
    return 0; //未能找到对应元素，返回0
    //算法时间复杂度：O(n)
}
~~~

***线性表的插入***

~~~cpp
bool InsertList(SqList& L, const ElemType &e, const int &i)
{
    //判断线性表长度是否小于最大长度MAXSIZE
    if(L.length == MAXSIZE)
    {
        cerr<<"can not insert!"<<endl;
        return false;
    }
    if(i<0 || i>L.length)
    {
        cerr << "wrong insert position!" << endl;
        return false;
    }
    if(L.length > 0)
    {
        //将位于插入位置之后的元素依次向后挪动一位
        for (int p = L.length - 1; p >= i; --p)
        {
            L.elem[p + 1] = L.elem[p];
        }
    }
    //插入元素
    L.elem[i] = e;
    //线性表长度+1
    L.length += 1;
    return true;
    //算法时间复杂度：O(n)
}
~~~

***线性表的删除***

~~~cpp
 bool EraseList(SqList& L, const int &i)
{
    //异常判断
    if(i<0 || i>L.length)
    {
        cerr << "wrong erase position!" << endl;
        return false;
    }
    if(L.length == 0)
    {
        cerr << "List has no length" << endl;
        return false;
    }
    //将位于删除位置之后的元素依次向前挪动一位
    for (int p = i + 1; p < L.length; ++p)
    {
        L.elem[p - 1] = L.elem[p];
    }
    //线性表长度-1
    L.length -= 1;
    return true;
    //算法时间复杂度：O(n)
}
~~~

**==链表==**

*链表的定义*

~~~cpp
struct Lnode
{
    ElemType data;//结点的数据域
    Lnode *next;//结点的指针域
};
~~~

