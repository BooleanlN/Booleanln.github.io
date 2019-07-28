---
title: 线性表
tags:
  - 数据结构
  - 线性表
url: 86.html
id: 86
categories:
  - 数据结构
date: 2018-06-19 15:34:11
---

#### 线性表的实现

###### 顺序存储

#include 
#include 
#define MAXSIZE 10
//数据结构
//线性表的顺序存储
 typedef struct LNode *List;
 struct LNode{
    int Data\[MAXSIZE\];
    int last;
};
//置空
List MakeEmpty()
{
    List Ptrl;
    Ptrl = (List)malloc(sizeof(struct LNode));
    Ptrl->last = -1;
    return Ptrl;
}
//查找指定下标节点
int Find(int X,List Ptrl)
{
    int i = 0;
    for(i=0;i<=Ptrl->last&&Ptrl->Data\[i\]!=X;i++);
    if(i>Ptrl->last) return -1;
    else return i;

}
/*链表尾插入*/
void insert(List Ptrl,int num)
{

    if(Ptrl->last<MAXSIZE-1) { Ptrl->last++;
        Ptrl->Data\[Ptrl->last\]=num;
        printf("last = %d\\n",Ptrl->last);
    }else{
        printf("cannot extend anymore\\n");
        return;
    }
    return Ptrl;
}
/*指定位置插入*/
void insertI(List Ptrl,int j,int num)
{
    if(Ptrl->last<MAXSIZE-1) { int i; Ptrl->last++;
        for(i=Ptrl->last;i>j;i--)
        {
            Ptrl->Data\[i\] = Ptrl->Data\[i-1\];
            //printf("前一个data=%d\\n",Ptrl->Data\[i-1\]);
        }
        Ptrl->Data\[j\]=num;

    }
    else{
        printf("cannot extend anymore\\n");

    }
}
//删除指定下标节点
void deleteL(List Ptrl,int j)
{
    if(Ptrl->last!=-1&&Ptrl->last>j&&j>=0)
    {
        int i;
        for(i=j;ilast;i++)
        {
            Ptrl->Data\[i\]=Ptrl->Data\[i+1\];
        }
        Ptrl->last--;
    }else{
    printf("something is wrong!\\n");
    }
}
void show(List Ptrl)
{
    if(Ptrl->last!=-1)
    {
        int i = 0;
        for(i=0;i<=Ptrl->last;i++)
        {
            printf("%d\\n",Ptrl->Data\[i\]);
        }
    }
}
int main()
{   List NList;
    NList = MakeEmpty();
    insert(NList,4);
    insert(NList,3);
    insert(NList,5);
    insert(NList,4);
    insert(NList,3);
    insert(NList,5);

    insertI(NList,2,6);
    show(NList);
    deleteL(NList,3);
    show(NList);
   // printf("%d",Find(5,NList));
    return 0;
}

###### 链式存储

#include 
#include 
#define MAXSIZE 10
//数据结构
//线性表的顺序存储
 typedef struct LNode *List;
 struct LNode{
    int Data\[MAXSIZE\];
    int last;
};
//置空
List MakeEmpty()
{
    List Ptrl;
    Ptrl = (List)malloc(sizeof(struct LNode));
    Ptrl->last = -1;
    return Ptrl;
}
//查找指定下标节点
int Find(int X,List Ptrl)
{
    int i = 0;
    for(i=0;i<=Ptrl->last&&Ptrl->Data\[i\]!=X;i++);
    if(i>Ptrl->last) return -1;
    else return i;

}
/*链表尾插入*/
void insert(List Ptrl,int num)
{

    if(Ptrl->last<MAXSIZE-1) { Ptrl->last++;
        Ptrl->Data\[Ptrl->last\]=num;
        printf("last = %d\\n",Ptrl->last);
    }else{
        printf("cannot extend anymore\\n");
        return;
    }
    return Ptrl;
}
/*指定位置插入*/
void insertI(List Ptrl,int j,int num)
{
    if(Ptrl->last<MAXSIZE-1) { int i; Ptrl->last++;
        for(i=Ptrl->last;i>j;i--)
        {
            Ptrl->Data\[i\] = Ptrl->Data\[i-1\];
            //printf("前一个data=%d\\n",Ptrl->Data\[i-1\]);
        }
        Ptrl->Data\[j\]=num;

    }
    else{
        printf("cannot extend anymore\\n");

    }
}
//删除指定下标节点
void deleteL(List Ptrl,int j)
{
    if(Ptrl->last!=-1&&Ptrl->last>j&&j>=0)
    {
        int i;
        for(i=j;ilast;i++)
        {
            Ptrl->Data\[i\]=Ptrl->Data\[i+1\];
        }
        Ptrl->last--;
    }else{
    printf("something is wrong!\\n");
    }
}
void show(List Ptrl)
{
    if(Ptrl->last!=-1)
    {
        int i = 0;
        for(i=0;i<=Ptrl->last;i++)
        {
            printf("%d\\n",Ptrl->Data\[i\]);
        }
    }
}
int main()
{   List NList;
    NList = MakeEmpty();
    insert(NList,4);
    insert(NList,3);
    insert(NList,5);
    insert(NList,4);
    insert(NList,3);
    insert(NList,5);

    insertI(NList,2,6);
    show(NList);
    deleteL(NList,3);
    show(NList);
   // printf("%d",Find(5,NList));
    return 0;
}

两个线性表的合并

List Merge(List L1,List L2)
{
    List L,head;
    if(L1->Data>L2->Data)
    {
        head = L2;
        L2 = L2->Next;
    }
    else
    {
        head = L1;
        L1 = L1->Next;
    }
    L = head;
    while(L1!=NULL&&L2!=NULL)
    {
        printf("L1 = %d L =%d L2 = %d\\n",L1->Data,L->Data,L2->Data);
        if(L2->DataData)
        {
            L->Next = L2;
            L = L2;
            L2 = L2->Next;
        }
        else{
            L->Next = L1;
            L = L1;
            L1 = L1->Next;
        }
    }
    if(L1)
    {
        L->Next = L1;
    }
    else if(L2){

        L->Next = L2;
    }
    return head;
}