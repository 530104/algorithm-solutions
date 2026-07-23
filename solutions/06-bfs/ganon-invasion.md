# 加农的入侵

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：广度优先搜索（BFS）

---

## 📝 题目描述

n 行 m 列的矩阵，`.` 草地，`*` 大石头。加农从起点向 8 个方向扩散，每步一个单位时间。问最远能到达的距离（即所有可达格子的最大步数）。

**输入**：先列数 m 行数 n，再起点坐标（数学坐标：先列后行），然后是 n 行地图。输入数据行列是反的，起点坐标也是数学坐标。

---

## 💡 思路

8 方向 BFS：起点入队后逐层扩散，`dist` 数组记录每个可达格子到起点的最短距离。遍历过程中不断更新最大距离 `res`。BFS 按层扩展保证每个格子第一次访问时距离最短。

---

## 🔑 关键代码

```cpp
const int N = 110;
char g[N][N];
int dist[N][N];
int n, m;
PII start;
// 8个方向：上下左右 + 四个对角线
const int dx[8] = {1,-1,0,0,1,-1,1,-1};
const int dy[8] = {0,0,1,-1,1,-1,-1,1};

int bfs() {
    memset(dist, -1, sizeof dist);       // -1 表示未访问
    queue<PII> q;
    q.push(start);
    dist[start.first][start.second] = 0; // 起点距离为0
    int res = 0;

    while (q.size()) {
        auto t = q.front(); q.pop();
        for (int i = 0; i < 8; i++) {    // 8个方向搜索
            int x = t.first + dx[i], y = t.second + dy[i];
            if (x < 1 || x > n || y < 1 || y > m) continue;  // 越界
            if (g[x][y] == '*' || dist[x][y] != -1) continue; // 障碍或已访问
            dist[x][y] = dist[t.first][t.second] + 1; // 距离+1
            res = max(res, dist[x][y]);   // 更新最大距离
            q.push(make_pair(x, y));      // 入队
        }
    }
    return res;
}
```

---

## 📝 总结

8 方向 BFS 与 4 方向只有方向数组的区别。题目输入格式特殊（行列颠倒 + 数学坐标），需要仔细处理坐标映射。

---

