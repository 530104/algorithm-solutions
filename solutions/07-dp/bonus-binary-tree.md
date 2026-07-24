# 加分二叉树

> **难度**：困难 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：动态规划

---

## 📝 题目描述

一棵 n 个节点的二叉树，中序遍历为 (1,2,...,n)，每个节点有分数 di。子树加分 = 左子树加分 x 右子树加分 + 根的分数，空子树加分为 1。求加分最高的二叉树及前序遍历。

**输入**：第一行 n（n < 30），第二行 n 个整数（分数 < 100）。

**输出**：第一行最高加分，第二行前序遍历。

**输入样例**：
```
5
5 7 1 2 10
```

**输出样例**：
```
145
3 1 2 4 5
```

---

## 💡 思路

区间 DP + 存储根节点：`f[l][r]` = 中序遍历区间 [l,r] 构成的二叉树的最大加分。枚举根 k，`f[l][r] = max(f[l][k-1] * f[k+1][r] + w[k])`。用 `root[l][r]` 记录使加分最大的 k，最后 DFS 输出前序遍历。

---

## 🔑 关键代码

```cpp
const int N = 50;
int n, w[N];
unsigned f[N][N];                  // f[l][r] = 区间[l,r]的最大加分
int root[N][N];                    // root[l][r] = f[l][r]取最大时的根节点

void dfs(int l, int r) {           // 根据root数组输出前序遍历
    if (l > r) return;
    int k = root[l][r];
    printf("%d ", k);              // 先输出根
    dfs(l, k - 1);                 // 再左子树
    dfs(k + 1, r);                 // 再右子树
}

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) scanf("%d", &w[i]);

    for (int len = 1; len <= n; len++)
        for (int l = 1; l + len - 1 <= n; l++) {
            int r = l + len - 1;
            for (int k = l; k <= r; k++) {        // 枚举根节点
                int left  = (k == l) ? 1 : f[l][k-1];  // 左子树加分
                int right = (k == r) ? 1 : f[k+1][r];  // 右子树加分
                int score = left * right + w[k];        // 当前树的加分
                if (l == r) score = w[k];               // 叶子节点
                if (f[l][r] < score) {
                    f[l][r] = score;
                    root[l][r] = k;                     // 记录最优根
                }
            }
        }

    printf("%d\n", f[1][n]);
    dfs(1, n);
    return 0;
}
```

---

## 📝 总结

区间 DP + 二叉树的结合。`root[l][r]` 存储最优决策是区间 DP 输出方案的常用技巧。空子树加分为 1 的设定简化了边界处理。

---

