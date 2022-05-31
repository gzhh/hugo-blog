---
title: "常见排序算法" 
date: 2018-04-01T23:24:03+08:00
draft: false
tags: [quick-sort]
categories: [Algorithm]
---
<!--more-->

### 简介：八大排序算法

插入排序【直接插入排序和希尔排序】

交换排序【冒泡排序和快速排序】

选择排序【选择排序和堆排序】

归并排序

基数排序


### 一、直接插入排序

假设这里有一数组

    n = 9;
    a[n] = {6,7,1,9,5,2,8,3,4};
    

    // 从当前位置向前遍历
    6
    6 7
    1 6 7
    1 6 7 9
    1 5 6 7 9
    1 2 5 6 7 9
    1 2 5 6 7 8 9
    1 2 3 5 6 7 8 9
    1 2 3 4 5 6 7 8 9
    

### 二、希尔排序

希尔排序，也称递减增量排序算法，是插入排序的一种更高效的改进版本。希尔排序是非稳定排序算法。 希尔排序是基于插入排序的以下两点性质而提出改进方法的：

插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位

性能：
没有快速排序的 O(N * Log(N)) 的快，但是比冒泡排序、选择排序和直接插入排序的 O(N * N) 要快很多

例子：

假设这里有一数组

    n = 9;
    a[n] = {6,7,1,9,5,2,8,3,4};
    

**第一次循环** 那么可以假设步长为 gap = n / 2 = 4，按步长进行分组

    a[0] = 6, a[4] = 5, a[8] = 4
    a[1] = 7, a[5] = 2
    a[2] = 1, a[6] = 8
    a[3] = 9, a[7] = 3
    

各个组之间进行排序

    a[0] = 4, a[4] = 5, a[8] = 6
    a[1] = 2, a[5] = 7
    a[2] = 1, a[6] = 8
    a[3] = 3, a[7] = 9
    

排序后的数组为 `4 2 1 3 5 7 8 9 6` **第二次循环** 步长再除以2得 gap = 2，排序后的数组为 `1 2 4 3 5 7 6 9 8` **第三次循环** 步长再除以2得 gap = 1，这时候就是直接插入排序，排序后的数组为 `1 2 3 4 5 6 7 8 9` **最后步长为0跳出循环** **代码**

    #include<iostream>
    using namespace std;
    // 直接插入排序
    void insertSort(int a[], int n) {
      for (int i = 1; i < n; i++) {
        int j = i, m = a[i];
          while (a[j-1] > m) {
            a[j] = a[j-1];
            j--;
          }
          a[j] = m;
      }
    }
    // 希尔排序(最小增量排序)
    void shellSort(int a[], int n) {
      int gap = n / 2; // 步长
      int i, j, temp;
      while (gap > 0) {
        for (i = gap; i < n; i++) {
          temp = a[i];
          for (j = i - gap; j >= 0; j -= gap) {
            if (a[j] > temp) {
              a[j + gap] = a[j]; // 向后移一个步长
            } else {
              break;
            }
          }
          a[j + gap] = temp;
        }
        gap /= 2; // 减小步长
    
        // 打印每次按步长排序的数组
        for (int m = 0; m < n; m++)
          cout << a[m] << " ";
        cout << endl;
      }
    }
    int main() {
      int n = 9;
      int a[n] = {6,7,1,9,5,2,8,3,4};
      // insertSort(a, n);
      shellSort(a, n);
      for (int i = 0; i < n; i++)
        cout << a[i] << " ";
      cout << endl;
    }


### 三、冒泡排序

原理：遍历内层数组，每次比较相邻的两个数，前者比后者大则交换，这样一轮下来，每次可以得到一个最大的数并放在尚未排好序元素的后面，直到排好序

    void bubbleSort(int a[], int len)
    {
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len - i - 1; j++)
                if (a[j] > a[j+1])
                    swap(a[j], a[j+1]);
        }
    }
    

### 四、快速排序

原理：取数组的第一个数为标志数，每次递归的目标就是按标志数将数组分为两部分，生成的新数组的左边那些数比标志数小，右边那些数比标志数大，然后递归下去，直到数据只剩下一个元素的时候

    void quickSort(int a[], int low, int high)
    {
        if (low >= high)
            return;
        int key = a[low];
        int i = low, j = high;
    
        while (i < j) {
            while (i < j && a[j] > key) j--;
            a[i] = a[j];
            while (i < j && a[i] < key) i++;
            a[j] = a[i];
        }
        a[i] = key;
        quickSort(a, low, i - 1);
        quickSort(a, i + 1, high);
    }

### 五、选择排序

原理：将第一个元素的索引设置为标志，遍历数组，将每个数和标志处的元素比较，若小于标志数，则将标志数设为当前数的索引，一轮遍历完，最后将标志数处的数和数组开头的数交换，继续遍历直至排好序

<!--more-->
    void selectSort(int a[], int len)
    {
        int key;
        for (int i = 0; i < len; i++) {
            key = i;
            for (int j = i + 1; j < len; j++) {
                if (a[j] < a[key])
                    key = j;
            }
            swap(a[i], a[key]);
        }
    }
    

### 六、堆排序

堆的定义：所有结点都比它左右子树的所有结点的值都大或者都小
完全二叉树的定义：平衡二叉树(叶子结点的高度差小于等于1)的一种特例，所有叶子结点都像左靠
原理： 首先可以将数组看成完全二叉树的层次遍历，然后我们先将叉树调整为最大堆，然后将堆根结点的值和堆最后一个叶子结点的交换，堆长度减一，继续调整堆，直到排好序

    // 调整堆
    void max_heapify(int a[], int start, int end)
    {
        int dad = start;
        int son = dad * 2 + 1;
        if (son > end) return;
    
        if (a[son] < a[son+1] && son + 1 <= end)
            son++;
    
        if (a[son] > a[dad]) {
            swap(a[son], a[dad]);
            // 继续调整子孙树
            max_heapify(a, son, end);
        }
    }
    
    // 堆排序
    void heapSort(int a[], int len)
    {
        // 1.从最后一个叶子结点的父结点开始遍历，将二叉树调整为一个最大堆
        for (int i = len / 2 - 1; i >= 0; i--) {
            max_heapify(a, i, len - 1);
        }
    
        // 2.将树根的值和最后一个结点的值交换，堆元素的长度减一，
        // 然后继续调整堆，这次只是微调
        for (int i = len - 1; i > 0; i--) {
            swap(a[0], a[i]);
            max_heapify(a, 0, i - 1);
        }
    }


### 七、归并排序
原理：
1.将序列每相邻两个数字进行归并操作，形成`ceil(n/2)`个序列， 排序后每个序列包含两/一个元素
2.若此时序列数不是1个则将上述序列再次归并，形成`ceil(n/4)`个序列， 每个序列包含四/三个元素
3.重复步骤2，直到所有元素排序完毕，即序列数为1 实现：

    #include<iostream>
    using namespace std;
    void merge_sort_recursive(int a[], int b[], int start, int end) {
      if (start >= end) // 如果待排序局部数组的长度为1,返回上一步递归调用的地方
        return;
      int len = end - start, mid = (len >> 1) + start; // 当前区间的中间值
      int start1 = start, end1 = mid; // 左区间
      int start2 = mid + 1, end2 = end; // 右区间
      merge_sort_recursive(a, b, start1, end1);
      merge_sort_recursive(a, b, start2, end2);
    
      int k = start; //记录当前区间开始的位置
      // 将两个区间合并为一个有序的区间
      while (start1 <= end1 && start2 <= end2)
        b[k++] = a[start1] < a[start2] ? a[start1++] : a[start2++];
      // 将其中一个区间中剩余的数添加到已排好序区间的后面
      while (start1 <= end1)
        b[k++] = a[start1++];
      while (start2 <= end2)
        b[k++] = a[start2++];
      // 将排好序的临时区间赋值给原先区间
      for (int i = start; i <= end; i++)
        a[i] = b[i];
    }
    
    void merge_sort(int a[], int len) {
      int b[len]; // 新建一个临时数组，用来存放局部数组的临时排序值
      merge_sort_recursive(a, b, 0, len - 1);
    }
    
    int main() {
      int a[9] = {5, 7, 1, 9, 6, 2, 8, 3, 4};
      int len = sizeof(a) / sizeof(a[0]);
      merge_sort(a, len);
      for (int i = 0; i < len; i++)
        cout << a[i] << " ";
      return 0;
    }

### 八、基数排序

[基数排序](https://zh.wikipedia.org/wiki/%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F)
