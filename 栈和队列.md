### 栈和队列

|  Part I   |   Part II   |
| :-------: | :---------: |
| [栈](#p1) | [队列](#p2) |

<span id="p1">**栈**</span>

**1-顺序栈**

*顺序栈的定义*

~~~cpp
struct SqStack
{
    ElemType *base;
    ElemType *top;
    int stacksize;
};
~~~

*顺序栈的初始化*

~~~cpp
bool InitStack(SqStack &S)
{
    S.base = new ElemType[MAXSIZE];
    if (!S.base)
    {
        cerr << "failed to get memory" << endl;
        return false;
    }
    S.top = S.base;
    S.stacksize = MAXSIZE;
    return true;
}
~~~

*压栈*

~~~cpp 
bool Push(SqStack &S, ElemType &e)
{
    //判断栈是否已满
    if((S.top-S.base) == S.stacksize)
    {
        cerr << "full of stack" << endl;
        return false;
    }
    *(S.top) = e;
    ++(S.top);
    return true;
}
~~~

*创建一个栈*

~~~cpp
bool CreatStack(SqStack &S, const int n)
{
    for (int i = 0; i < n;++i)
    {
        ElemType input;
        cin >> input;
        if(!Push(S,input))
        {
            cerr << "error happend at-" << i << endl;
            return false;
        }
    }
    return true;
}
~~~

*弹栈*

~~~cpp
bool Pop(SqStack &S, ElemType &e)
{
    if(!IsEmpty(S))
    {
        cerr << "empty stack!error" << endl;
        return false;
    }
    --(S.top);
    e = *(S.top);
    return true;
}
~~~

*判断顺序栈是否为空*

~~~cpp
bool IsEmpty(SqStack &S)
{
    if (S.base == S.top)
    {
        return true;
    }
    else
    {
        return false;
    }
}
~~~

*获取栈的元素个数*

~~~cpp
int StackLength(SqStack &S)
{
    return static_cast<int>(S.top - S.base);
}
~~~

*清空顺序栈*

~~~cpp
bool ClearStack(SqStack &S)
{
    if(S.base)
    {
        S.top = S.base;
        return true;
    }
    else
    {
        return false;
    }
}
~~~

*销毁顺序栈*

~~~cpp
void DestoyStack(SqStack &S)
{
    if(S.base)
    {
        delete[] S.base;
        S.stacksize = 0;
        S.base = S.top = nullptr;
    }
}
~~~

***  
**2-链式栈**

*链式栈的定义*

~~~cpp
typedef struct StackNode
{
    ElemType data;
    StackNode *next;
} * LinkStack;
~~~

*栈的初始化*

~~~cpp
void InitStack(LinkStack &S)
{
    //创建头结点
    S = new StackNode;
    S->next = nullptr;
    S->data = 0; //表示栈的元素个数
}
//王老师的视频中没有设置头结点
//由于ELemType等价于int，因此我设置了一个头结点，在头结点的数据域中保存栈的元素个数
~~~

*压栈*

~~~cpp
void Push(LinkStack &S, const ElemType &e)
{
    //插入元素
    StackNode *temp = new StackNode;
    temp->data = e;
    temp->next = S->next;
    S->next = temp;
    ++(S->data);//元素个数加一
}
~~~

*弹栈*

~~~cpp
void Pop(LinkStack &S, ElemType &e)
{
    //删除元素
    StackNode *p = S->next;
    e = p->data;
    S->next = p->next;
    --(S->data);
    delete p;
}
~~~

*创建栈*

~~~cpp
void CreatStack(LinkStack &S, const int n)
{
    ElemType input;
    for (int i = 0; i < n;++i)
    {
        cin >> input;
        Push(S, input);
    }
}
~~~

*获取栈的元素个数*

~~~
int StackLength(LinkStack &S)
{
    return S->data;
}
~~~

*判断栈是否为空*

~~~cpp
bool IsEmpty(LinkStack &S)
{
    if(!(S->next))
    {
        return true;
    }
    else 
    {
        return false;
    }
}
~~~

*清空栈*

~~~cpp
bool ClearStack(LinkStack &S)
{
    if(!(S->next))
    {
        cerr << "empty Stack" << endl;
        return false;
    }
    StackNode *q, *p = S->next;
    while(p)
    {
        q = p;
        p = p->next;
        delete q;
    }
    S->data = 0;
    S->next = nullptr;
    return true;
}
~~~

*销毁栈*

~~~cpp
bool DestoryStack(LinkStack &S)
{
    if(S)
    {
        cerr << "error" << endl;
        return false;
    }
    while(S)
    {
        StackNode *temp = S;
        S = S->next;
        delete temp;
    }
    return true;
}
~~~
  
***

<span id="p2">**队列**</span>

**1-顺序队列**

*顺序队列定义*

~~~cpp
struct SqQueue
{
    ElemType *elem;
    int front;
    int rear;
};
~~~

*顺序队列初始化*

~~~cpp
void InitQueue(SqQueue &Q)
{
    Q.elem = new ElemType[MAXSIZE];
    Q.front = Q.rear = 0;
}
~~~

*判断队是否为空*

~~~cpp
bool IsEmpty(SqQueue &Q)
{
    if (Q.front == Q.rear)
        return true;
    else
        return false;
}
~~~

*判断是否满队*

~~~cpp
bool IsFull(SqQueue &Q)
{
    auto rear_next = (Q.rear + 1) % MAXSIZE;
    if (rear_next == Q.front)
        return true;
    else
        return false;
}
~~~

*进队*

~~~cpp
bool InsertQueue(SqQueue &Q, const ElemType &e)
{
    //如果队尾+1等于队头，表明队已经满了（该队列是少用一个空间的循环队列，满队和空队的判断条件不一致）
    if (IsFull(Q))
    {
        cerr << "full of Queue" << endl;
        return false;
    }
    Q.elem[Q.rear] = e;
    Q.rear = (Q.rear + 1) % MAXSIZE;
    return true;
}
~~~

*批量进队*

~~~cpp
void CreatQueue(SqQueue &Q, const int n)
{
    cout << "input msg" << endl;
    ElemType input;
    for (int i = 0; i < n;++i)
    {
        cin >> input;
        InsertQueue(Q, input);
    }
}
~~~

*出队*

~~~cpp
bool EraseQueue(SqQueue &Q)
{
    //如果队头等于队尾，表明队里没有元素，不执行该程序
    if (IsEmpty(Q))
    {
        cerr << "no elem to erase" << endl;
        return false;
    }
    //e = Q.elem[Q.front];
    Q.front = (Q.front + 1) % MAXSIZE;
    return true;
}
~~~

***求队列的元素个数***

~~~cpp
int GetLength(SqQueue &Q)
{
    return (Q.rear - Q.front + MAXSIZE) % MAXSIZE;
}
~~~

*打印队列*

~~~cpp
void PrintQueue(SqQueue &Q)
{
    cout << "Queue:" << endl;
    for (auto i = Q.front; i != Q.rear; i = (i + 1) % MAXSIZE)
    {
        cout << Q.elem[i] << endl;
    }
}
~~~

***

**2-链式队列**

***定义链队***

~~~cpp
struct Qnode
{
    ELemType data;
    Qnode *next;
};
struct LinkQueue //再定义一个抽象数据类型，一次性建立两个Qnode指针
{
    Qnode *front; //头指针
    Qnode *rear;  //尾指针
};
~~~

*初始化*

~~~cpp
void InitQueue(LinkQueue &Q)
{
    Q.front = Q.rear = new Qnode;
    Q.front->data = 0; //用于保存链队的元素个数
    Q.rear->next = nullptr;
}
~~~

*进队*

~~~cpp
void InsertQueue(LinkQueue &Q, const ELemType &e)
{
    //把元素插在最后面
    Qnode *temp = new Qnode;
    temp->data = e;
    temp->next = nullptr;
    Q.rear->next = temp;
    Q.rear = temp;
    ++Q.front->data; //元素个数加1
}
~~~

*创建链队*

~~~cpp
void CreatQueue(LinkQueue &Q, const int n)
{
    cout << "input msg" << endl;
    ELemType input;
    for (int i = 0; i < n; ++i)
    {
        cin >> input;
        InsertQueue(Q, input);
    }
}
~~~

*出队*

~~~cpp
bool EraseQueue(LinkQueue &Q)
{
    if (Q.front->next == nullptr)
    {
        cerr << "empty Queue" << endl;
        return false;
    }
    Qnode *p = Q.front->next;
    Q.front->next = p->next;
    delete p;
    --Q.front->data; //元素个数-1
    return true;
}
~~~

*打印链队*

~~~cpp
void PrintQueue(LinkQueue &Q)
{
    Qnode *p = Q.front->next;
    while (p)
    {
        cout << p->data << endl;
        p = p->next;
    }
}
~~~
> 本篇完~
