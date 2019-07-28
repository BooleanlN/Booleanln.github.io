---
title: 数据结构概述 (Overview of data structure)
url: 83.html
id: 83
categories:
  - Other
  - 数据结构
date: 2018-06-19 09:33:34
tags:
---

### 什么是数据结构？

书架问题，怎么去摆放书，有多少种摆放方法？各种摆放方法有什么优势，劣势？

数的遍历问题：

两种遍历方法，对比：

void printN(int N);
int main()
{
 int n;
 scanf("%d",&n);
 printN(n);
 return 0;
}
//第一种
void printN(int N)
{
 int i = 1;
 for(i;i<=N;i++)
 {
 printf("%d\\n",i);
 }
 return;
}
//第二种
void printN(int N)
{
 if(N)
 {
 printN(N-1);
 printf("%d\\n",N);
 }
 return;
}
​

当输入N超过100000左右时，第二种递归方法会产生错误，跳出程序执行 结论：算法的优劣与空间的利用有关 两种计算多项式方法比较： 结论：算法优劣与时间效率有关 什么是数据结构？

*   数据对象在计算机中的组织方式——逻辑结构，物理存储结构
    
*   数据对象必定与一系列加在其上的操作相关联
    
*   完成这些操作所用的方法，即算法
    

### 什么是算法？

*   一个有限指令集
    
*   接受一些输入
    
*   产生输出
    
*   有限步骤终止
    

好的算法：

*   空间复杂度S(n)
    
*   时间复杂度T(n)
    
*   最坏情况复杂度Tworst(n)
    
*   平均复杂度Tavg(n)
    
*   复杂度的渐进表示法(复杂度从小到大)
    
    *   1
        
    *   log n
        
    *   n
        
    *   nlog n
        
    *   n*n
        
    *   n* n * n
        
    *   2^n
        
    *   n!
        

两段算法分别有复杂度T1 = O(f(n))和 T2 = O(f(n)) T1+T2 = max(O(f1(n)),O(f2(n))) T1*T2 = O(f1(n)xf2(n)) 一个for循环的时间复杂度等于循环次数乘以循环体内代码的复杂度 if-else结构的复杂度取决于if的条件判断复杂度和两个分支部分的复杂度，总体复杂度取三者中最大 最大子列和问题学习：

/\*1.在线解决方法：\*/
package leetcode;
​
import java.util.Scanner;
​
/**
 \* Created by Boolean on 2018/5/8.
 */
public class Solution2 {
 public static void main(String\[\] args)
 {
 Scanner scanner = new Scanner(System.in);
 int num = scanner.nextInt();
 int\[\] arr = new int\[num\];
 int i = 0;
 while(num--!=0) {
 arr\[i\] = scanner.nextInt();
 i++;
 }
 scanner.close();
 int sum = 0,currentsum=0;
 for(i=0;i<arr.length;i++)
 {
 currentsum+=arr\[i\];
 if(currentsum<0)
 {
 currentsum = 0;
 }else if(currentsum>sum){
 sum = currentsum;
 }
 }
 System.out.print(sum);
 }
}