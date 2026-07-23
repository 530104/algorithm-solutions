# 骑士林克的怜悯(2)

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：广度优先搜索（BFS）

---

## 📝 题目描述

在 n 行 m 列的棋盘上，`K` 表示骑士起点，`H` 表示守护者（终点），`*` 表示障碍，`.` 表示空地。骑士按"马走日"有 8 个跳跃方向。问从起点跳到终点的最少步数。

**输入**：先列数 m 行数 n，接着 n 行地图。

**输出**：最少步数，无法到达则输出 -1。

---

## 💡 思路

"马走日"BFS 求最短路径：8 个方向为马步，`dist` 数组记录最短距离。当搜索到终点坐标时直接返回当前距离，因为 BFS 按层搜索保证第一次到达即为最短路径。

---

## 🔑 关键代码

```cpp
const int N = 155;
char g[N][N];
int dist[N][N];
int n, m;

int bfs(PII start, PII end) {
    memset(dist, -1, sizeof dist);      // -1 表示未访问
    dist[start.first][start.second] = 0;
    queue<PII> q;
    q.push(start);

    // 马走日的8个方向
    int dx[] = {-2,-1,1,2,2,1,-1,-2};
    int dy[] = {1,2,2,1,-1,-2,-2,-1};

    while (q.size()) {
        auto t = q.front(); q.pop();
        for (int i = 0; i < 8; i++) {   // 8个马步方向
            int x = t.first + dx[i], y = t.second + dy[i];
            if (x < 0 || x >= n || y < 0 || y >= m) continue;  // 越界
            if (g[x][y] == '*' || dist[x][y] != -1) continue;  // 障碍/已访问
            dist[x][y] = dist[t.first][t.second] + 1; // 步数+1

            if (make_pair(x, y) == end)  // 到达终点，立即返回
                return dist[x][y];
            q.push({x, y});             // 入队继续搜索
        }
    }
    return -1;                          // 无法到达终点
}
```

---

## 📝 总结

骑士巡游 BFS 版。与 DFS 版(Knight's Tour)不同，BFS 求最短路径，DFS 求遍历所有格子的路径。马步方向数组是 2×4 的 L 形位移。

---

