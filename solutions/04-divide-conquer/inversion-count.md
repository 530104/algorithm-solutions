# 求排列的逆序数

> **难度**：困难 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：分治算法

---

## 📝 题目描述

给定 1~n 的一个排列，求它的逆序数。逆序：若 j < k 且 a_j > a_k，则 (a_j, a_k) 是一个逆序对。n <= 100000。

**输入**：第一行 n，第二行 n 个不同正整数。

**输出**：逆序数。

**输入样例**：
```
6
2 6 3 4 5 1
```

**输出样例**：
```
8
```

---

## 💡 思路

归并排序天然适合求逆序数：在合并两个有序子数组时，若 `nums[p] > nums[q]`，说明左半 `[p, mid]` 中剩余的所有数都大于 `nums[q]`，每个都与 `nums[q]` 构成一个逆序对，贡献 `mid - p + 1` 个逆序。归并过程累加左右子区间的逆序数即可。

---

## 🔑 关键代码

```cpp
long long merge_sort(int nums[], int left, int right) {
    if (left >= right) return 0;                     // 递归边界：区间长度 <=1，逆序数为0
    int mid = left + right >> 1;

    // 分：递归求左半和右半的逆序数，累加
    long long result = merge_sort(nums, left, mid) + merge_sort(nums, mid + 1, right);

    // 治：合并两个有序子数组，同时统计跨越左右的逆序对
    int k = 0, p = left, q = mid + 1;
    while (p <= mid && q <= right) {
        if (nums[p] <= nums[q]) {                    // 不构成逆序，左指针元素归入temp
            temp[k++] = nums[p++];
        } else {                                     // nums[p] > nums[q]：构成逆序
            result += mid - p + 1;                   // 左半 [p, mid] 所有数都 > nums[q]
            temp[k++] = nums[q++];                   // 右指针元素归入temp
        }
    }
    while (p <= mid) temp[k++] = nums[p++];          // 左半剩余元素
    while (q <= right) temp[k++] = nums[q++];        // 右半剩余元素

    for (int i = left, k = 0; i <= right; i++, k++)  // 拷贝回原数组
        nums[i] = temp[k];
    return result;
}
```

---

## 📝 总结

归并排序求逆序对是关键面试考点。核心理解：当右指针元素被合并时（比左半当前数小），左半剩余的所有数都比它大，因此每个都构成一个逆序对。时间复杂度 O(n log n)。

---

