# Dijkstra 求最短路 (1)

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：广度优先搜索（BFS）

---

## 📝 题目描述

给定 n 个点 m 条边的有向图（可能存在重边和自环，所有边权为正），求 1 号点到 n 号点的最短距离。若不可达输出 -1。

**输入**：第一行 n m，接下来 m 行每行 x y z（从 x 到 y 的有向边，边权 z）。

**输出**：最短距离或 -1。

**输入样例**：
```
3 3
1 2 2
2 3 1
1 3 4
```

**输出样例**：
```
3
```

---

## 💡 思路

Dijkstra 朴素版（O(n^2)）：维护 `dist` 数组记录 1 到各点的最短距离。每次从未确定最短距离的点中选出 `dist` 最小的点 t，标记为已确定，然后用 t 松弛所有邻接点。重复 n-1 次。适用于稠密图（n <= 500）。

---

## 🔑 关键代码

```cpp
const int N = 510;
int n, m;
int g[N][N];                 // 邻接矩阵存图（稠密图）
int dist[N];                 // dist[i] = 1号点到i号点的最短距离
bool st[N];                  // st[i] = true 表示i的最短距离已确定

int dijkstra() {
    memset(dist, 0x3f, sizeof dist); // 初始化为无穷大
    dist[1] = 0;                     // 起点距离为0

    for (int i = 0; i < n - 1; i++) {        // 循环 n-1 次
        int t = -1;
        for (int j = 1; j <= n; j++)          // 找未确定点中 dist 最小的
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        st[t] = true;                         // 标记 t 的距离已确定
        for (int j = 1; j <= n; j++)          // 用 t 松弛所有邻接点
            dist[j] = min(dist[j], dist[t] + g[t][j]);
            // 若通过 t 到 j 更近，更新 dist[j]
    }
    if (dist[n] == 0x3f3f3f3f) return -1;     // 不可达
    return dist[n];
}
```

---

## 📝 总结

Dijkstra 是贪心 + 松弛的最短路算法。朴素版核心：每次选 `dist` 最小的未确定点作为中转点进行松弛。邻接矩阵适合稠密图；稀疏图可用堆优化降到 O(m log n)。

---

