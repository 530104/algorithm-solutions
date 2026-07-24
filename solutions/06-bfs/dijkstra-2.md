# Dijkstra 求最短路 (2)

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：广度优先搜索（BFS）

---

## 📝 题目描述

给定 n 个点 m 条边的有向图（可能存在重边和自环，所有边权非负），求 1 号点到 n 号点的最短距离。若不可达输出 -1。n, m 可达 150000（稀疏图）。

**输入**：第一行 n m，接下来 m 行每行 x y z。

**输出**：最短距离。

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

堆优化 Dijkstra（O(m log n)）：用优先队列（小根堆）替代每次遍历所有点找最小值。每次从堆顶取出距离最小的点 u，若已被处理则跳过；否则标记已处理，遍历 u 的所有出边进行松弛。稀疏图效率远优于朴素 O(n^2)。

---

## 🔑 关键代码

```cpp
typedef pair<int, int> pii;            // {距离, 点编号}
const int N = 150005;
const int INF = 0x3f3f3f3f;

int n, m;
vector<pii> g[N];                      // 邻接表：g[u] = {v, w}
int dist[N];
bool vis[N];                           // 标记点是否已确定最短距离

void dijkstra(int s) {
    memset(dist, 0x3f, sizeof(dist));  // 初始化为无穷大
    dist[s] = 0;
    // 小根堆：每次取距离最小的未处理点
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    pq.push({0, s});

    while (!pq.empty()) {
        auto [d, u] = pq.top();        // 取出堆顶（当前最短距离的点）
        pq.pop();
        if (vis[u]) continue;          // 已处理过，跳过（处理重边/旧记录）
        vis[u] = true;                 // 标记已确定

        for (auto [v, w] : g[u]) {     // 遍历 u 的所有出边
            if (dist[v] > d + w) {     // 松弛：通过 u 到 v 更近
                dist[v] = d + w;
                pq.push({dist[v], v}); // 新距离入堆
            }
        }
    }
}
// 答案：dist[n] == INF ? -1 : dist[n]
```

---

## 📝 总结

堆优化的核心是用 `priority_queue` 实现 O(log n) 取最小值，代替朴素版的 O(n) 扫描。注意同一个点可能多次入堆（不同的松弛路径），`vis` 数组确保每个点只处理一次。

---

