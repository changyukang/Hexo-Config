title: 冒泡排序(Bubble Sort)
layout: post
comment: true
date: 2016-03-11 23:25:51
categories: [Programming,Algorithm_and_Data_Structure,Sorting]
tags: [Algorithm,Sorting]
keywords: compare sorting, unstable sorting,bubble

description: <div class="note info"><p>sorting algorithm about bubble sort</p></div>
---

# 冒泡排序(Bubble Sort)
## 1. 算法思想
冒泡排序（Bubble Sort）也是一种简单直观的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢"浮"到数列的顶端。

冒泡排序还有一种优化算法，就是设置一个 flag，当在一趟序列遍历中元素没有发生交换，则证明该序列已经有序。但这种改进对于提升性能来
说并没有什么太大作用。
![](resources/bubbleSort.gif)

最好情况：有序序列，不需要交换元素，但是内层两两比较还是需要的，此时算法复杂度还是$O(n^2)$，但是设置了 flag 后，有一趟遍历发现没有两两交换发生，说明序列有序，此时对于有序序列第一次就能将 flag 置位，此时算法复杂度将为了$O(n+1)= O(n)$；

最差情况：反向序列，每两个元素都要进行交换；

冒泡排序**稳定与否与实现有关**，如果只有在 a[i]>a[i+1]时才进行交换，那么就是稳定的，否则，如果a[i]=a[i+1]也交换，假设有两个相等元素且它们是当前序列中最大的，那么排在前面的将在第一次冒泡排到数组最后位置，之后那个最大的排在数组倒数第二位，这样二者顺序就颠倒了，就不稳定了。

## 2. 算法实现
```C
#include <stdio.h>
void bubble_sort(int arr[], int len) {
        int i, j, temp;
        for (i = 0; i < len - 1; i++)
                for (j = 0; j < len - 1 - i; j++)
                        if (arr[j] > arr[j + 1]) {
                                temp = arr[j];
                                arr[j] = arr[j + 1];
                                arr[j + 1] = temp;
                        }
}
int main() {
        int arr[] = { 22, 34, 3, 32, 82, 55, 89, 50, 37, 5, 64, 35, 9, 70 };
        int len = (int) sizeof(arr) / sizeof(*arr);
        bubble_sort(arr, len);
        int i;
        for (i = 0; i < len; i++)
                printf("%d ", arr[i]);
        return 0;
}
```