title: 选择排序(Selection Sort)
layout: post
comment: true
date: 2016-03-13 22:12:36
categories: [Programming,Algorithm_and_Data_Structure,Sorting]
tags: [Algorithm,Sorting]
keywords: compare sorting, unstable sorting,selection_sort

description: <div class="note info"><p>sorting algorithm about selection sort</p></div>
---

# 选择排序(Selection Sort)
## 1. 算法思想
选择排序(Selection-sort)是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

默认第一个元素问最小元素,并记录当前元素;
扫描第一个之后的所有元素和最小元素相比较如果小于最小元素把此元素作为最小元素一趟选择排序下来,找到最小元素下标,如果最小元素和当前元素不相等,则交换当前元素和最小元素；
这样第一位元素有序,重复以上步骤;

![](resources/selectionSort.gif)

```
原始数据
34 54  12  78 3  45  9  

I = 0;

minIndex = 0;
minIndex = 2;
minIndex = 4;

3  54  12  78 34  45  9  

I = 1;
minIndex = 1;
minIndex = 2;
minIndex = 6;

3  9  12  78 34  45  54 

I = 2;
minIndex = 2;

I = 3;
minIndex = 3;
minIndex = 4;

3  9  12 34  78  45  54 

I = 4;
minIndex = 4;
minIndex = 5;

3  9  12 34  45  78  54 


I = 5;
minIndex = 5;
minIndex = 6;

3  9  12 34  45 54  78
```

## 2. 算法实现
```C++
template<typename T> //可用于整型、浮点型，如果用于 class，需要自定义比较函数（运算符重载）
void selection_sort(std::vector<T>& arr) {
        for (int i = 0; i < arr.size() - 1; i++) {
                int min = i;
                for (int j = i + 1; j < arr.size(); j++)
                        if (arr[j] < arr[min])
                                min = j;
                std::swap(arr[i], arr[min]);
        }
}
```