title: 归并排序(Merge Sort)
layout: post
comment: true
date: 2016-06-17 12:16:19
categories: [Programming,Algorithm_and_Data_Structure,Sorting]
tags: [Algorithm,Sorting]
keywords: compare sorting, stable sorting

description: <div class="note info"><p>sorting algorithm about merge sort</p></div>
---

# 归并排序(Merge Sort)
## 1. 算法思想
归并排序（Merge sort）是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。

作为一种典型的分而治之思想的算法应用，归并排序的实现由两种方法：
1. 自上而下的递归（所有递归的方法都可以用迭代重写，所以就有了第 2 种方法）；
2. 自下而上的迭代；

和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是 O(nlogn) 的时间复杂度。代价是需要额外的内存空间。

## 2. 算法步骤
1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
设定两个指针，最初位置分别为两个已经排序序列的起始位置；
2. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
3. 重复步骤 3 直到某一指针达到序列尾；
4. 将另一序列剩下的所有元素直接复制到合并序列尾。

![IMAGE](resources/08EBE5F47F531C52173EB941A0E5D831.jpg)
递归深度为$log2n$。

逐层合并过程：
![IMAGE](resources/CE490D0DEAA4D0B3531264033AC25728.jpg)

## 3. 算法实现
```Java
public class MergeSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        if (arr.length < 2) {
            return arr;
        }
        int middle = (int) Math.floor(arr.length / 2);

        int[] left = Arrays.copyOfRange(arr, 0, middle);
        int[] right = Arrays.copyOfRange(arr, middle, arr.length);

        return merge(sort(left), sort(right));
    }

    protected int[] merge(int[] left, int[] right) {
        int[] result = new int[left.length + right.length];
        int i = 0;
        while (left.length > 0 && right.length > 0) {
            if (left[0] <= right[0]) {
                result[i++] = left[0];
                left = Arrays.copyOfRange(left, 1, left.length);
            } else {
                result[i++] = right[0];
                right = Arrays.copyOfRange(right, 1, right.length);
            }
        }

        while (left.length > 0) {
            result[i++] = left[0];
            left = Arrays.copyOfRange(left, 1, left.length);
        }

        while (right.length > 0) {
            result[i++] = right[0];
            right = Arrays.copyOfRange(right, 1, right.length);
        }

        return result;
    }

}
```

```C++
// Iteration version
template<typename T> 
void merge_sort(T arr[], int len) {
    T *a = arr;
    T *b = new T[len];
    for (int seg = 1; seg < len; seg += seg) {
        for (int start = 0; start < len; start += seg + seg) {
            int low = start, mid = min(start + seg, len), high = min(start + seg + seg, len);
            int k = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while (start1 < end1 && start2 < end2)
                b[k++] = a[start1] < a[start2] ? a[start1++] : a[start2++];
            while (start1 < end1)
                b[k++] = a[start1++];
            while (start2 < end2)
                b[k++] = a[start2++];
        }
        T *temp = a;
        a = b;
        b = temp;
    }
    if (a != arr) {
        for (int i = 0; i < len; i++)
            b[i] = a[i];
        b = a;
    }
    delete[] b;
}

// Recursion version
void Merge(vector<int> &Array, int front, int mid, int end) {
    // preconditions:
    // Array[front...mid] is sorted
    // Array[mid+1 ... end] is sorted
    // Copy Array[front ... mid] to LeftSubArray
    // Copy Array[mid+1 ... end] to RightSubArray
    vector<int> LeftSubArray(Array.begin() + front, Array.begin() + mid + 1);
    vector<int> RightSubArray(Array.begin() + mid + 1, Array.begin() + end + 1);
    int idxLeft = 0, idxRight = 0;
    LeftSubArray.insert(LeftSubArray.end(), numeric_limits<int>::max());
    RightSubArray.insert(RightSubArray.end(), numeric_limits<int>::max());
    // Pick min of LeftSubArray[idxLeft] and RightSubArray[idxRight], and put into Array[i]
    for (int i = front; i <= end; i++) {
        if (LeftSubArray[idxLeft] < RightSubArray[idxRight]) {
            Array[i] = LeftSubArray[idxLeft];
            idxLeft++;
        } else {
            Array[i] = RightSubArray[idxRight];
            idxRight++;
        }
    }
}

void MergeSort(vector<int> &Array, int front, int end) {
    if (front >= end)
        return;
    int mid = (front + end) / 2;
    MergeSort(Array, front, mid);
    MergeSort(Array, mid + 1, end);
    Merge(Array, front, mid, end);
}
```

## 4. 快速排序和归并排序时间复杂度计算上的差别
1. 对于归并排序，计算中间值：$O(1)$，之后将数据二分，这个过程等如果不使用将数据拷贝到新的数组，而仅仅是在原数组操作索引的话，就相当于啥都没做就分好区了，而这种计算中间值分区的操作一共要进行$logn$次；分到最后，逐层网上归并，每次归并时间复杂度是$O(n)$，虽然在最底层只有两个元素，但是复杂度要考虑一般情况，而每次往上，需要归并的元素越多；总体上是$O(nlogn)$
2. 对于快速排序，每次选用第一个元素做 pivot：$O(1)$，之后分区的动作就不想归并那么简单啥都不做了，而是需要双向右左遍历数组，将大的放 pivot 右侧，小的放 pivot 左侧，这个过程每次要双向遍历数组，时间复杂度是$O(n)$;这种分区要进行$logn$次，总体上时间复杂度是：$O(nlogn)$