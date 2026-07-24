# 最长上升子序列

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：动态规划

---

## 📝 题目描述

给定长度为 N 的数列，求数值严格递增的最长子序列长度。N <= 100000。

**输入**：第一行 N，第二行 N 个整数。

**输出**：最长上升子序列长度。

**输入样例**：
```
7
3 1 2 1 8 5 6
```

**输出样例**：
```
4
```

---

## 💡 思路

**O(n^2)**：`f[i]` 以 a[i] 结尾的 LIS 长度，`f[i] = max(f[j] + 1)`，其中 a[j] < a[i]。

**O(n log n)**：维护数组 `q[]`，`q[len]` 表示长度为 len 的上升子序列的最小末尾值。遍历 a[i] 时二分查找 q 中最后一个小于 a[i] 的位置 r，更新 `q[r+1] = a[i]`。

---

## 🔑 关键代码

```cpp
// O(n log n) 二分优化
const int N = 100010;
int n, a[N], q[N];               // q[len] = 长度为len的LIS的最小末尾值

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);

    int len = 0;
    for (int i = 0; i < n; i++) {
        int l = 0, r = len;      // 二分查找最后一个 < a[i] 的位置
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (q[mid] < a[i]) l = mid;
            else r = mid - 1;
        }
        len = max(len, r + 1);   // r+1 是以a[i]结尾的LIS长度
        q[r + 1] = a[i];         // 更新该长度的最小末尾值
    }
    printf("%d\n", len);
    return 0;
}
```

---

## 📝 总结

LIS 是线性 DP 经典题。O(n^2) 解法直观易懂；O(n log n) 解法中 `q[]` 数组始终单调递增，因此可用二分查找加速。

---

