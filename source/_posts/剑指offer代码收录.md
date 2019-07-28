---
title: 剑指offer代码收录
date: 2019-02-13 22:33:12
tags:
- 编程练习
---

### 剑指offer题目代码收录

1. **面试题3**

   ```java
   import java.util.*;
   
   /**
    * Created by 13633 on 2019/1/19.
    * @desc 输入一个二维数组，该数组从左至右，从上至下递增，输入该数组，再输入一个数，在这个数组中
    * 找到该数字，返回是否找到
    */
   public class Main {
       public static void main(String[] args){
          int m,n;
          Scanner scanner = new Scanner(System.in);
          m = scanner.nextInt();
          n = scanner.nextInt();
          int matrix[][] = new int[m][n];
          for(int i=0;i<m;i++)
              for (int j=0;j<n;j++)
                  matrix[i][j] = scanner.nextInt();
          int num = scanner.nextInt();
          boolean isFound = false;
          for(int i=n-1;i>=0;i--) {
              if(matrix[0][i]>num)continue;
              for (int j = 0; j < m; j++) {
                   if(matrix[j][i]==num) {
                       System.out.println("位置在(" + (i + 1) + "，" + (j + 1) + ")");
                       isFound = true;
                   }
                   if(matrix[j][i]>num)break;
              }
          }
          if(!isFound) System.out.println("没 有 找 到 ！");
       }
   }
   
   ```

2. 面试题4

   ```java
   import java.util.*;
   
   /**
    * Created by 13633 on 2019/1/19.
    * @desc 输入一个字符串，将字符串中空格换成%20
    * 采用由后往前置换
    */
   public class Main {
       public static void main(String[] args){
           Scanner scanner = new Scanner(System.in);
           String str = scanner.nextLine();
           int len = str.length();
           int space_n = 0;
           for(int i=0;i<len;i++)
           {
               if(str.charAt(i)==' ')
                   space_n++;
           }
   
           int new_len = len+space_n*2;
           char[] new_str = new char[new_len];
           int index = new_len-1;
           for(int i=len-1;i>=0;i--)
           {
               if(str.charAt(i)==' ')
               {
                   new_str[index] = '0';
                   new_str[index-1] = '2';
                   new_str[index-2] = '%';
                   index-=3;
               }
               else
               {
                   new_str[index] = str.charAt(i);
                   index--;
                 //  System.out.println(index);
               }
           }
           System.out.println(new_str);
       }
   }
   
   ```

3. 字符串leetcode

   - 无重复字符的最长子串

     采用滑动窗口机制以及hashmap或数组来做字典映射：

     复杂度最低：

     ```java
     public class Main {
         public static int lengthOfLongestSubstring(String s) {
           int[] chars = new int[128];
           int maxlen = 0,left = 0,right = 0;
           int n = s.length();
           for (right = 0; right < n;right++) {
               char ch = s.charAt(right);
               //若新字符在字典中已出现，则表明，须从该字符第一次出现的地方开始重新计数，则
               //left取该字符第一次出现时的数字，right-left+1则为新串长度
               if(chars[ch]>left)left = chars[ch];
               //右窗口外延，长度增加
               chars[ch] = right+1;
               //长度对比
               maxlen = maxlen>(right-left+1)?maxlen:(right-left+1);
           }
           return maxlen;
         }
         public static void main(String[] args) {
             System.out.println( lengthOfLongestSubstring("abcabcvv"));
         }
     }
     ```

     自己写的：

     ```java
     class Solution {
         public int lengthOfLongestSubstring(String s) {
            int[] chars = new int[128];
           int maxlen = 0;
           for (int i = 0,j=0; j <s.length() ;) {
               if(chars[s.charAt(j)]==0)
               {
                   chars[s.charAt(j++)]++;
                   maxlen = Math.max(maxlen,j-i);
               }else {
                   chars[s.charAt(i++)] = 0;
               }
           }
           return maxlen;
         }
     }
     ```
