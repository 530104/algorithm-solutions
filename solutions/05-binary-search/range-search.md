# 攻击范围

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：二分算法

---

## 📝 题目描述

在升序数组 nums 中（可能有重复元素），对于每个查询 target，输出 target 在数组中出现的起始和终止位置（从 0 开始）。找不到输出 `-1 -1`。n <= 100000, q <= 10000。

**输入**：第一行 n q，第二行 n 个整数，接下来 q 行每行一个 k。

**输出**：q 行，每行两个整数表示起始和终止位置，或 `-1 -1`。

**输入样例**：
```
6 3
1 2 2 3 3 4
3
4
5
```

**输出样例**：
```
3 4
5 5
-1 -1
```

---

## 💡 思路

分别用两种二分模板找左边界和右边界。左边界：`p[mid] >= target` 时 `r = mid`；右边界：`p[mid] <= target` 时 `l = mid`。先找左边界，若 `p[left] != target` 则不存在，否则继续找右边界。

---

## 🔑 关键代码

```cpp
// 找左边界：第一个 >= target 的位置
int Left(int l, int r, int target) {
    while (l < r) {
        int mid = l + (r - l) / 2;       // 等价于 (l+r)>>1，防溢出
        if (p[mid] >= target)
            r = mid;                     // 答案在左半（含mid）
        else
            l = mid + 1;                 // 答案在右半
    }
    return l;                            // l == r，即左边界
}

// 找右边界：最后一个 <= target 的位置
int Right(int l, int r, int target) {
    while (l < r) {
        int mid = l + (r - l + 1) / 2;   // 上取整，避免死循环
        if (p[mid] <= target)
            l = mid;                     // 答案在右半（含mid）
        else
            r = mid - 1;                 // 答案在左半
    }
    return r;                            // r == l，即右边界
}
// 注意：Right 的 mid 要上取整，否则只剩两个数且 p[mid]<=target 时会死循环
```

---

## 📝 总结

二分求区间范围是两个模板的组合使用。核心区别：`mid` 下取整时要 `l = mid + 1`，上取整时要 `r = mid - 1`，否则会死循环。

---

