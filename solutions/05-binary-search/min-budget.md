# 最小预算值

> **难度**：困难 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：二分算法

---

## 📝 题目描述

将 N 天的固定支出分为 M 组（每组连续），每组分配固定预算 Budget（各组预算相同，结余不可挪用）。求能覆盖所有组支出的最小 Budget。1 <= N <= 100000，1 <= M <= N。

**输入**：第一行 N M，第二行 N 个整数（每天支出）。

**输出**：最小预算值。

**输入样例**：
```
7 5
100 400 300 100 500 101 400
```

**输出样例**：
```
500
```

---

## 💡 思路

二分答案 Budget：二分的下界为每天支出的最大值（至少要有这么多才能覆盖单天），上界为全部支出之和（分 1 组）。`check(mid)` 模拟贪心分组：从左到右累加，若超过 mid 则另起一组，若分组数 > M 则 mid 不可行。最小可行值即为答案。

---

## 🔑 关键代码

```cpp
int cost[100005];
int N, M;

bool check(int x) {                     // 检验 Budget=x 是否可行
    int cnt = 1;                        // 当前已分组数
    int subtotal = 0;                   // 当前组累计支出
    for (int i = 0; i < N; i++) {
        if (cost[i] > x) return false;  // 某一天本身就超出预算，x 不可能
        if (subtotal + cost[i] > x) {   // 加上当天会超出预算
            cnt++;                      // 新开一组
            subtotal = cost[i];         // 重置当前组，当天作为新组第一天
            if (cnt > M) return false;  // 组数超标，x 不可行
        } else {
            subtotal += cost[i];        // 当天留在当前组
        }
    }
    return cnt <= M;                    // 分组数不超过 M 则可行
}

int bsearch(int l, int r) {
    while (l < r) {
        int mid = (l + r) / 2;          // 下取整二分
        if (check(mid))                 // mid 可行，尝试更小的值
            r = mid;
        else                            // mid 不可行，需要更大的值
            l = mid + 1;
    }
    return l;                           // l == r 时即为答案
}
```

---

## 📝 总结

二分答案的核心：将优化问题转化为判定问题（给定 Budget，能否满足条件）。贪心判定 O(n)，二分 O(n log V)，总效率远优于暴力枚举。

---

