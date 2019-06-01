title: 希尔排序（ShellSort）
layout: post
comment: true
date: 2016-06-11 16:21:55
categories: [Programming,Algorithm_and_Data_Structure,Java, Sorting]
tags: [Java,Algorithm,Data Structure,Sorting]
keywords: compare sorting, unstable sorting
description: Introduction to data structure and some basic concepts
---


# 希尔排序（ShellSort）

希尔排序按其设计者希尔（Donald Shell）的名字命名。希尔排序通过多次插入排序来实现。它通过比较相距一定间隔的元素来工作；各趟比较所用的距离随着算法的进行而减小，直到只比较相邻元素的最后一趟排序为止。所以希尔排序也叫缩减增量排序。

## 2. 算法思想：
使用一个序列h1,h2,….ht,叫做增量序列。任何增量序列都是可行的，前提是 h1 必须等于 1。在使用增量hk排序后，对于每一个i 我们都有a[i]<=a[i+hk]；所有相隔hk的元素都被排序，这称为hk排序。只要最后h1=1（这时就是最普通的插入排序）,希尔排序都可完成工作。一趟hk排序就是对hk个子数组进行插入排序。

## 3. 排序过程
1. 选择一个增量序列h1,h2,….ht，h1=1。
2. 根据增量序列个数，即循环t次进行排序，每次排序结束后更换为ht-1的增量。
3. 把原数组分为ht个子数组，对每个子数组进行插入排序。
下面我们使用增量序列1、3、5对序列 {3, 7, 1, 13, 9, 11, 5, 8, 2, 4, 12, 6, 10}进行希尔排序的图解。每种颜色代表一个子数组，很直观看到每一趟排序过程及结果。
![希尔排序图解](resources/545C88C7D43AF4986918317D1880381F.jpg)

    java实现冒泡排序：代码中我们使用希尔建议的增量序列（效率不高）,ht=N/2和hk=hk+1/2。
    ```Java
     private static <T extends Comparable<? super T>> void shellsort(T[] a) {
            int j;
            T tmp = null;
            for (int gap = a.length / 2; gap > 0; gap /= 2) {
                for (int i = gap; i < a.length; i++) {
                    tmp = a[i];
                    for (j = i; j >= gap && tmp.compareTo(a[j - gap]) < 0; j -= gap) {
                        a[j] = a[j - gap];
                    }
                    a[j] = tmp;
                }
            }
        }
    ```
## 4. 时间、空间复杂度及稳定性分析：
希尔排序的运行时间依赖于增量序列的选择，而证明很复杂【有兴趣可查看其他资料】。
使用希尔增量时希尔排序最坏时间复杂度是：O(n^2)。
使用Hibbard增量的希尔排序最坏时间复杂度是：O(n3/2)；最优时间复杂度是O(n5/4)。
使用Sedgewick 增量序列,排序最坏时间复杂度是：O(n4/3)；平均时间复杂度是O(n7/6)。最好的序列是{1,5,19,41,109……}。该序列中的项或者是9 * 4i - 9 * 2i +1的形式，或者是4i - 3* 2i+1)的形式。
空间复杂度：只用到一个临时变量，所以空间复杂度为O(1)；
稳定性：不稳定排序。因为每一趟的步长不一样，所以步长长的插入排序可能会把后面的元素插入到前面。

## 5. 总结
希尔排序通过使用增量序列来将原始序列分为多个子序列，对每个子序列进行插入排序。只要最终增量序列h1=1,希尔排序都可正常工作。希尔排序时间严重依赖于增量序列的选择，我们可以直接先将好的增量序列表存在数组中，这样不用每次排序都去计算。
