### 线性表

|      Part I       |     Part II     |    Part III     |     Part IV     |       Part V        |
| :---------------: | :-------------: | :-------------: | :-------------: | :-----------------: |
| [顺序线性表](#p1) | [单向链表](#p2) | [双向链表](#p3) | [循环链表](#p4) | [三个案例分析](#p5) |

|            |                            顺序表                            |                             链表                             |
| :--------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  存储空间  |              预先分配，可能会导致空间闲置或溢出              |              动态分配，不会出现空间闲置或者溢出              |
|  存储密度  |       存储密度为1，逻辑关系等于存储关系，没有额外开销        |     存储密度小于1，要借助指针域来表示元素之间的逻辑关系      |
|  存取元素  |           随机存取，按位置访问元素的时间复杂度O(1)           |         顺序存取，访问某位置的元素的时间复杂度为O(n)         |
| 插入、删除 | 插入和删除都要移动大量的元素。平均移动元素约为表的一半。时间复杂度O(n) | 不需要移动元素，只需要改变指针位置，继而改变结点之间的链接关系。时间复杂度O(1) |
|  适用情况  | 1.表长变化不大，或者事先就能确定变化的范围<br />2.很少进行插入和删除，需要下标访问元素 |            1.长度变化较大<br />2.频繁的插入和删除            |
|            |                 *这不就是vector的使用特点吗*                 |                  *这不就是list的使用特点吗*                  |

<span id="p1">**==1. 顺序线性表==**</span>

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
bool InitList(SqList &L)
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
void DestroyList(SqList &L)
{
    if(L.elem)
    {
        delete L.elem;
    }
}
~~~

*线性表的清空*

~~~cpp
void CLearList(SqList &L)
{
    L.length = 0;
}
~~~

*判断线性表是否为空*

~~~cpp
bool IsEmpty(const SqList &L)
{
    return static_cast<bool>(L.length);
}
~~~

***线性表的取值***

~~~cpp
bool GetELem(const SqList &L, const size_t i, ElemType &e)
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
int LocateList(const SqList &L, const ElemType &e)
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
bool InsertList(SqList &L, const ElemType &e, const int &i)
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
bool EraseList(SqList &L, const int &i)
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



<span id="p2">**==单向链表==**</span>

*链表的定义*

~~~cpp
struct Lnode
{
    ElemType data;//结点的数据域
    Lnode *next;//结点的指针域
};
typedef Lnode *LinkList;
~~~

*链表的初始化*

~~~cpp
bool InitList(LinkList &L)//插入题外话：LinkList &L等价于 Lnode *&L，Lnode *&L是一个指向指针的引用
{
    L = new Lnode; //堆区开辟一个头结点，结点的数据类型为Lnode
    L->next = nullptr;  //空表，也就是说头结点的指针指向为空
    return true;
}
~~~

*头插法创建单向链表*

~~~cpp
void CreatListHead(LinkList &L, const size_t n)
{
    for (int i = 0; i < n; ++i)
    {
        Lnode *p = new Lnode;
        cin >> p->data;
        p->next = L->next;
        L->next = p; 
    }
}
~~~

*尾插法创建单向链表*

~~~cpp
void CreatListTail(LinkList &L, const size_t n)
{
    Lnode *r = L;
    for (int i = 0; i < n; ++i)
    {
        Lnode *p = new Lnode;
        cin >> p->data;
        p->next = r->next;
        r->next = p;
        r = r->next;
    }
}
~~~

*判断链表是否为空*

~~~cpp
bool IsEmpty(const LinkList &L)
{
    if(L->next)//非空
    {
        return false;
    }
    else 
    {
        return true;
    }
}
~~~

*销毁链表*

~~~cpp
bool DestroyList(LinkList &L)
{
    //判断链表是否为空
    if(IsEmpty(L))
    {
        cerr << "empty List!" << endl;
        return false;
    }
    while (L)//链表还未到达尾端
    {
        auto temp = L->next;//将头指针指向下一个结点
        delete L;
        L = temp;
    }
    return true;
}
~~~

*统计链表长度*

~~~cpp
size_t GetLength(const LinkList &L)
{
    Lnode *p;
    p = L->next;
    size_t cnt = 0;
    while (p)
    {
        ++cnt;
        p = p->next;
    }
    return cnt;
}
//算法的时间复杂度为O(n)
~~~

*取链表中第i个元素的值*

~~~cpp
bool GetElem(const LinkList &L, const int &i, ElemType &e)
{
    if(i < 0)
    {
        cerr << "out of range" << endl;
        return false;
    }
    Lnode *p = L->next;
    for (int j = 1; j < i + 1; ++j)
    {
        p = p->next;
        if(!p)
        {
            cerr << "out of range" << endl;
            return false;//如果此时p为空，意味着已经到达链表尾端，停止循环
        }
    }
    e = p->data;
    return true;
}
~~~

***按值查找链表***

~~~cpp
size_t LocateElem(LinkList &L, ElemType &e)
{
    Lnode *p = L->next;
    size_t cnt = 1;
    while (p)
    {
        if (p->data == e)
        {
            return cnt;
        }
        ++cnt;
        p = p->next;
    }
    cerr << "not found" << endl;
    return 0;
}
~~~

***在链表中插入元素***

~~~cpp
bool InsertList(LinkList &L, const int &i, const ElemType &e)
{
    Lnode *p = L;
    int j = 0;
    while(p && j < i-1)
    {
        p = p->next;
        ++j;
    }
    //异常判断
    if(!p || i<0)
    {
        cerr << "out of range" << endl;
        return false;
    }
    LinkList insert = new Lnode;
    insert->data = e;
    insert->next = p->next;
    p->next = insert;
    return true;
}
//算法的时间复杂度为O(n)
~~~

***删除链表的某个结点***

~~~cpp
bool EraseList(LinkList &L, const int &i)
{
    Lnode *p = L;
    int j = 0;
    while (p->next && j < i - 1)
    {
        p = p->next;
        ++j;
    }
    if (!(p->next) || i < 0)
    {
        cerr << "out of range" << endl;
        return false;
    }
    Lnode *q = p->next;
    p->next = p->next->next;
    delete q;
    return true;
}
~~~

***两个有序链表的合并***

~~~cpp
void MergeList(LinkList &La, LinkList &Lb, LinkList &Lc)
{
    Lnode *pa = La->next;
    Lnode *pb = Lb->next;
    Lc = La;
    Lnode *pc = Lc;
    while (pa && pb)
    {
        if (pa->data <= pb->data) //尾插法，插入元素
        {
            //pc的指针域指向小元素的地址
            pc->next = pa;
            //移动pc指针，使得pc永远都指向最后链表Lc的最后一个元素
            pc = pc->next;
            //pa的元素使用过后，要向后移动pa
            pa = pa->next;
        }
        else
        {
            //pc的指针域指向小元素的地址
            pc->next = pb;
            //移动pc指针，使得pc永远都指向最后链表Lc的最后一个元素
            pc = pc->next;
            //pb的元素使用过后，要向后移动pa
            pb = pb->next;
        }
    }
    //上面的while循环执行完毕后，较长的链表还会余留一段元素，这段元素的起始地址就是pa（或pb
    pc->next = (pa ? pa : pb);
    //链表合并完毕，释放Lb的头结点
    delete Lb;
}
~~~

> ​		这个算法的时间复杂度为O(n)，但是空间复杂度为O(1)
> ​		自我感觉，这个算法的思想十分巧妙。La和Lb是两条有序链表，众所周知链表的元素的逻辑关系是通过指针域实现的。这个算法巧妙的地方在于：不需要在堆区（heap）申请新的内存空间组成合并链表，而就根据原有元素的地址，重新构建一组逻辑关系。总而言之，就是通过改变现有结点指针的指向，构造出一条新的链表

***稀疏多项式的相加***

> 也即“合并两个有序链表”的变形

~~~cpp
void SPO_II(LinkList &La, LinkList &Lb, LinkList &Lc)
{
    Lnode *pa = La->next;
    Lnode *pb = Lb->next;
    Lc = La;
    Lnode *pc = Lc;
    while(pa && pb)
    {
        if(pa->data.index < pb->data.index)
        {
            pc->next = pa;
            pc = pc->next;
            pa = pa->next;
        }
        else if(pa->data.index > pb->data.index)
        {
            pc->next = pb;
            pc = pc->next;
            pb = pb->next;
        }
        else if(pa->data.index == pb->data.index)
        {
            pa->data.coef += pb->data.coef;
            pc->next = pa;
            pc = pc->next;
            pa = pa->next;
            pb = pb->next;
        }
    }
    pc->next = (pa ? pa : pb);
    delete Lb;
}
~~~

***

<span id="p3">**循环链表**</span>

*循环链表的定义*

~~~cpp
typedef struct CLnode
{
    ElemType data;
    CLnode *next;
}*CircList;
~~~

*循环链表的初始化*

~~~cpp
void InitList(CircList &L)
{
    L = new CLnode;
    L->next = L;
}
~~~

==循环链表的基本操作和单链表基本上相同，唯一不同的是，由于循环链表的最后一个结点的next不再是空指针，而是指向头结点，因此，循环中的结束条件要发生变化==

~~~cpp
单链表--------------循环链表
while(p)--------->while(p!=L)
while(p->next)--->while(p->next!=L)
~~~



<span id="p4">**双向链表**</span>

*双向链表的定义*

~~~cpp
typedef struct DuLnode
{
    ElemType data;
    DuLnode *prior, *next;
} * DuLinkList;
~~~

*双向链表的初始化*

~~~cpp
void InitList(DuLinkList &L)
{
    L = new DuLnode;
    L->prior = nullptr;
    L->next = nullptr;
}
~~~

*头插法创建双向链表*

~~~cpp
void CreatListHead(DuLinkList &L, const size_t n)
{
    for (int i = 0; i < n; ++i)
    {
        DuLnode *p = new DuLnode;
        cin >> p->data;
        p->prior = L;
        p->next = L->next;
        L->next = p;
    }
}
~~~

*尾插法创建双向链表*

~~~cpp
void CreatListTail(DuLinkList &L, const size_t n)
{
    DuLnode *r = L;
    for (int i = 0; i < n; ++i)
    {
        DuLnode *p = new DuLnode;
        cin >> p->data;
        p->prior = r;
        p->next = r->next;
        r->next = p;
        r = p;
    }
}
~~~

***在双向链表的第i个位置插入元素***

~~~cpp
bool ListInsert_DuL(DuLinkList &L, const int i, const ElemType &e)
{
    //移动指针到i处
    DuLnode *p = L->next;
    int j = 1;
    while (p->next && j < i)
    {
        ++j;
        p = p->next;
    }
    if (j < i || j < 1) //如果i在链表范围内，上面的while循环的终止条件就是j<i
    {
        cerr << "out of range" << endl;
        return false;
    }
    //在堆区创建要插入的结点
    DuLnode *s = new DuLnode;
    s->data = e;
    //重新建立链接关系
    s->prior = p->prior; //第一步：s的前趋等于p的前趋
    p->prior->next = s;  //第二步，用p的前趋结点的next指向插入元素s，更改了第一条链
    s->next = p;         //第三步：s的后继指向p
    p->prior = s;        //第四步：p的前趋改为指向s，更改了第二条链
    //return
    return true;
}
~~~

***删除双向链表中的某个元素***

~~~cpp
bool ListErase_DuL(DuLinkList &L, const int i)
{
    //移动指针到i处
    DuLnode *p = L->next;
    int j = 1;
    while (p->next && j < i)
    {
        ++j;
        p = p->next;
    }
    if (j < i || j < 1) //如果i在链表范围内，上面的while循环的终止条件就是j<i
    {
        cerr << "out of range" << endl;
        return false;
    }
    //改变链接关系
    p->prior->next = p->next;//p的前趋结点的next等于p的后继
    if ((p->next))//如果删除的不是最后一个元素
    {
        p->next->prior = p->prior;
    }
    //释放p
    delete p;
    //结束
    return true;
}
~~~

***

<span id="p5">**三个案例分析**</span>

> 案例一：一元多项式运算
>
> 案例二：稀疏多项式运算
>
> 案例三：图书馆管理系统

***案例一：一元多项式的合并 -顺序链表版***

~~~cpp
void PolyOperate(SqList &L1, SqList &L2, SqList &L3)
{
    for (int i = 0; i < L1.length && i < L2.length; ++i)
    {
        L3.elem[i] = L1.elem[i] + L2.elem[i];
        L3.length += 1;
    }
    if (L1.length <= L2.length)
    {
        for (int j = L1.length; j < L2.length; ++j)
        {
            L3.elem[j] = L2.elem[j];
            L3.length += 1;
        }
    }
    else
    {
        for (int j = L2.length; j < L1.length; ++j)
        {
            L3.elem[j] = L1.elem[j];
            L3.length += 1;
        }
    }
}
~~~

***稀疏多项式的相加***

> 也即“合并两个有序顺序表”的变形

~~~cpp
void SQL_I(SqList &L1, SqList &L2, SqList &L3)
{
    ElemType *p1 = L1.elem;
    ElemType *p1_last = L1.elem + L1.length - 1;
    ElemType *p2 = L2.elem;
    ElemType *p2_last = L2.elem + L2.length - 1;
    ElemType *p3 = L3.elem;
    while (p1 <= p1_last && p2 <= p2_last)
    {
        if (p1->index < p2->index)
        {
            p3->index = p1->index;
            p3->coef = p1->coef;
            ++p1;
            ++p3;
            ++L3.length;
        }
        else if (p1->index > p2->index)
        {
            p3->index = p2->index;
            p3->coef = p2->coef;
            ++p2;
            ++p3;
            ++L3.length;
        }
        else if (p1->index == p2->index)
        {
            p3->index = p1->index;
            p3->coef = p1->coef + p2->coef;
            ++p1;
            ++p2;
            ++p3;
            ++L3.length;
        }
    }
    while (p1 <= p1_last)
    {
        p3->index = p1->index;
        p3->coef = p1->coef;
        ++p1;
        ++p3;
        ++L3.length;
    }
    while (p2 <= p2_last)
    {
        p3->index = p2->index;
        p3->coef = p2->coef;
        ++p2;
        ++p3;
        ++L3.length;
    }
}
~~~

***稀疏多项式的相加***

> 也即“合并两个有序链表”的变形

~~~cpp
void SPO_II(LinkList &La, LinkList &Lb, LinkList &Lc)
{
    Lnode *pa = La->next;
    Lnode *pb = Lb->next;
    Lc = La;
    Lnode *pc = Lc;
    while(pa && pb)
    {
        if(pa->data.index < pb->data.index)
        {
            pc->next = pa;
            pc = pc->next;
            pa = pa->next;
        }
        else if(pa->data.index > pb->data.index)
        {
            pc->next = pb;
            pc = pc->next;
            pb = pb->next;
        }
        else if(pa->data.index == pb->data.index)
        {
            pa->data.coef += pb->data.coef;
            pc->next = pa;
            pc = pc->next;
            pa = pa->next;
            pb = pb->next;
        }
    }
    pc->next = (pa ? pa : pb);
    delete Lb;
}
~~~

***图书管理系统 - 单链表实现***

*结构体定义*

~~~cpp
typedef struct Book
{
    string isbn;
    string name;
    float price;
} ElemType;
struct Lnode
{
    ElemType data;
    Lnode *next;
} *LinkList;
~~~

其他操作，例如对图书的添加、删除、查找等操作，和单链表基本上一样的，这里就不赘述了。不过，受到《C++ Primer》的启发，我们可以添加两个这样的函数，简化程序：

~~~cpp
//使用read函数向ElemType的对象写入数据
istream &read(istream &in, ElemType &rhs)
{
    in>>rhs.isbn;
    in>>rhs.name;
    in>>rhs.price;
    return in;
}
~~~

~~~cpp
//使用print函数打印ElemType对象
ostream &print(ostream &out, ElemType &rhs)
{
    out<<rhs.isbn<<" "
        <<rhs.name<<" "
        <<rhs.price<<endl;
    return out;
}
~~~

如何使用这两个函数？

~~~cpp 
//读
read(cin, L->data);
//写
print(cout, L->data);
~~~

本篇完~
