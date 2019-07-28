---
title: 堆（heap）
tags:
  - heap
  - 堆
  - 数据结构
url: 74.html
id: 74
categories:
  - 数据结构
date: 2018-06-18 13:52:43
---

**概念** **优先队列：特殊的队列，取出元素的顺序依照元素的优先权大小，而不是元素进入队列的先后顺序** 如何实现优先队列：数组？有序数组？链表？有序链表？二叉搜索树？ 方法：使用数组表示的完全二叉树来表示，任何一个结点都是其子树所有节点的最大值（或最小值） 从任意路径都可以保证有序性 **ADT：** 类型名称：**最大堆（MAXHeap）** 数据对象：**完全二叉树，每个结点的值不小于其子树的任意一个结点的元素值** 操作集： `1.MaxHeap Create（int MaxSize） 2.Boolean IsFull(MaxHeap Heap) 3.void Insert(MaxHeap heap,ElementType Item) 4.Boolean IsEmpty(heap) 5.ElementType DeleteMax(MaxHeap heap)` 代码实现： 堆的声明： `struct MaxHeap{ int *data; int MaxSize; int size; }; typedef struct MaxHeap* mxheap;` 1.堆的创建： `mxheap CreateHeap(int MaxSize) { mxheap heap = (struct MaxHeap*)malloc(sizeof(struct MaxHeap)); heap->data = (int *)malloc(sizeof(int)*MaxSize); heap->size = 0; heap->MaxSize = MaxSize; return heap; }` 2.非空、非满 `int IsFull(mxheap heap) { return (heap->size+1==heap->MaxSize); } int IsEmpty(mxheap heap) { return (heap->size==0); }` 3.插入元素 `void Insert(mxheap heap,int item) { if(IsFull(heap)) { printf("堆已满\n"); return; } int i = ++heap->size; //从下往上遍历。直到堆顶 //判断i>1，否则跳出，若当前位置的父节点小于要插入的结点，则将当前结点改为父节点值，并更改i值，直到找到合适的位置 for(;i>1&&heap->data[i/2]<item;i/=2) { heap->data[i] = heap->data[i/2]; } //赋值 heap->data[i]=item; // printf("%d位置插入data=%d\n",heap->size,heap->data[heap->size]); }` 4.删除元素 `int Delete(mxheap heap) { //空堆否？ if(IsEmpty(heap)) { printf("堆为空!\n"); return; } //保存堆顶值 int MaxItem = heap->data[1]; //获取最后一个结点的值 int FinData = heap->data[heap->size--]; int parent,child; //遍历，调整当前堆为最大堆 for(parent = 1;parent*2<=heap->size;parent=child) { child = parent*2; //若有两个子树，判断哪个更大 if(child!=heap->size&&heap->data[child]data[child+1])child++; //若当前位置子树的结点值小于最后的值，则找到插入位置 if(FinData>=heap->data[child]){break;} //否则，当前位置的值为两个结点中的最大值 else { heap->data[parent]=heap->data[child]; } } //若放在函数体中，当size=1时，函数返回错误，所以将其放在外面 heap->data[parent]=FinData; return MaxItem; }` 5.遍历元素 `//遍历输出Heap； void ShowHeap(mxheap heap) { while(!IsEmpty(heap)) { printf("%d\n",Delete(heap)); } }` 6.测试 `int main() { mxheap heap = CreateHeap(10); Insert(heap,5); Insert(heap,10); Insert(heap,1); Insert(heap,8); Insert(heap,50); Insert(heap,25); Insert(heap,17); Insert(heap,-5); Insert(heap,6); Insert(heap,64); ShowHeap(heap); return 0; }` 测试结果： `堆已满 50 25 17 10 8 6 5 1 -5 Process returned 0 (0x0) execution time : 0.008 s Press any key to continue.`