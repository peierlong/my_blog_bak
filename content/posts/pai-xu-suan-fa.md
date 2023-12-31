---
title: 排序算法
date: 2021-06-13
tags:
  - Java
---


## 排序算法执行效率

最好、最坏、平均时间复杂度

时间复杂度的系数、常数、低阶（规模很小的数据）

比较次数和交换（移动）次数

## 排序算法的内存消耗

也就是所谓的空间复杂度。 针对排序算法的空间复杂度，引入了**原地排序（sort in place）**的概念。

## 排序算法的稳定性

所谓稳定性，就是在原数据中存在相同的值，在经过排序后相同值的位置保持顺序不变。

## 排序算法

### 冒泡排序(停留在理论，很少用到了)

相邻两个元素进行比较，大的元素向后推，经过多次循环，数据有序。

```python
def bubble_sort(a):
    n = len(a)
    for i in range(n - 1):
        b = False        
for j in range(n - i - 1):            
    if a[j] > a[j+1]:               
        a[j], a[j + 1] = a[j + 1], a[j]                
        b = True        
    if not b:            
        break
```

- 原地排序

- 稳定排序算法

- 最好时间复杂度 O(n)

- 最坏时间复杂度 O(n²)

- 平均时间复杂度 O(n²)

### 插入排序（经常会用到）

从下标位1的数据开始遍历，拿出数据，向前遍历到插入到合适的位置。

```python
def insertionSort(a):    
for i in range(1, len(a)):
for j in range(i, 0, -1):
  if a[j] < a[j - 1]:
    a[j], a[j - 1] = a[j - 1], a[j]
  else:
    break
```

- 原地排序

- 稳定排序算法

- 最好时间复杂度 O(n)

- 最坏时间复杂度 O(n²)

- 平均时间复杂度 O(n²)

### 选择排序（停留在理论，很少会用到了）

依次遍历，把最小值依次向前放。

```python
def selectionSort(a):    
n = len(a)    
for i in range(n - 1):        
min_idx = i        
for j in range(i, n):            
if a[j] < a[min_idx]:                
  min_idx = j        
  a[i], a[min_idx] = a[min_idx], a[i]
```

- 原地排序

- 不稳定排序算法

- 最好时间复杂度 O(n²)

- 最坏时间复杂度 O(n²)

- 平均时间复杂度 O(n²)

### 并归排序

并归排序使用的是分治思想，把数组分成两部分，先拆分，后合并。

伪代码：

```python
// 归并排序算法，A 是数组，n 表示数组大小
merge_sort(A, n){
merge_sort_c(A, 0, n-1)
}
  // 递归调用函数
merge_sort_c(A, p, r){
// 递归终止条件
if p >= r then return
  // 取 p 到 r 之间的中间位置 q
q = (p + r) / 2
// 分治递归
merge_sort_c(A, p, q)
merge_sort_c(A, q+1, r)
// 将 A[p ... q] 和 A[q+1 ... r] 合并为 A[p...r]
merge(A[p...r], A[q...q], A[q+1...r])
}
```

```python
merge(A[p...r], A[p...q], A[q+1...r]){
// 初始化变量 i, j, k
var i := p, j := q+1, k := 0
var tmp := new array[0...r-p]
while i<=q and j<=r do {
if A[i] <= A[j] {
tmp[k++] = A[i++]
}else {
tmp[k++] = A[j++]
}
}
// 判断那个子数组中有剩余的数据
var start := i, end :=q
if j<=r then start := j, end := r
// 将剩余的数据拷贝到临时数组 tmp
while start <= end do {
tmp[k++] = A[start++]
}
// 讲 tmp 中的数组拷贝回 A[p...r]
for i := 0 to r-p do {
A[p+i] = tmp[i]
}
}
```

- 非原地排序

- 稳定排序算法

- 时间复杂度 O(nlogn)

### 快速排序

从数组中选择任意一个数据作为分区点（pivot），然后遍历数据，小于pivot的放到左边，大于的放到右边。 多次重复此动作，直到数据有序。

```python
// 快速排序，A 是数组，n 表示数组的大小
quick_sort(A, n){
quick_sort_c(A, 0, n-1)
}
// 快速排序递归函数，p, r 为下标
quick_sort_c(A, p, r){
if p >= r then return
  q = partition(A, p, r)
quick_sort_c(A, p, q-1)
quick_sort_c(A, q+1, r)
}
```

id:: 63565c74-0827-492d-9e77-ed5af5011211

```python
partition(A, p, r){
pivot := A[r]
i := p
for j := p to r-1 do {
if A[j] < pivot {
    swap a[i++] with A[j]
}
}
swap A[i] with A[r]
return i
}
```

- 原地排序

- 非稳定排序算法

- 平均时间复杂度 O(nlogn)

- 最坏时间复杂度 O(n²)

### 桶排序

依次遍历所有数据，然后把数据顺序放入到分割好的顺序桶中，然后在依次遍历桶就ok了。

- 时间复杂度 O(n)

场景

- 只能用在数据范围不大的场景中，要排序的数据很容易划分成m个桶。

- 各个桶之间分布的数据要比较均匀（如果不均匀还需要再分割）

- 桶排序比较适合用在外部排序中

### 计数排序 (Counting sort)

在一个数组中记录每个数据出现的次数，然后遍历原数组，依次计算出每个数据在有序数组中的位置。

- 时间复杂度 O(n)

场景：

- 计数排序只能用在数据范围不大的场景中。

- 计数排序只能排序非负整数，在某些情况下需要进行转换。

### 基数排序 (Radix sort)

排序的重点是利用稳定排序算法，先对最后一位排序，然后依次向前一位一位的进行排序。

- 时间复杂度 O(n)

场景

- 需要分隔出独立的位来比较，而且位之间有递进关系。

- 每一位的数据范围不能太大要可以用线性排序算法来排序。

```python
def selectionSort(a):
n = len(a)
for i in range(n - 1):
		min_idx = i
for j in range(i, n):
		if a[j] < a[min_idx]:
				min_idx = j
				a[i], a[min_idx] = a[min_idx], a[i]
```
