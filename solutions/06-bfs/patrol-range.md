# 波克布林的巡逻范围

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：广度优先搜索（BFS）

---

## 📝 题目描述

波克布林在 m 行 n 列的方格中巡逻，从 (0,0) 出发。只能进入横纵坐标各位数字之和 <= k 的格子。问总共能到达多少个格子。例如 k=18 时可进入 (35,37)（3+5+3+7=18），不能进入 (35,38)（3+5+3+8=19）。

**输入**：k m n。

**输出**：可到达格子数。

---

## 💡 思路

BFS 从 (0,0) 开始向四个方向扩展，对每个新坐标计算数位和，若 <= k 且未访问则入队。用 `vector<vector<bool>>` 标记已访问。

---

## 🔑 关键代码

```cpp
int get_int_sum(int x) {           // 计算整数x各位数字之和
    int s = 0;
    while (x) { s += x % 10; x /= 10; }
    return s;
}

int get_pair_sum(pair<int, int> p) {  // 计算坐标(x,y)的各位数字之和
    return get_int_sum(p.first) + get_int_sum(p.second);
}

int bfs(int threshold, int rows, int cols) {
    if (!rows || !cols) return 0;
    vector<vector<bool>> st(rows, vector<bool>(cols, false));
    queue<pair<int, int>> q;
    int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
    int res = 0;

    q.push({0, 0});
    while (q.size()) {
        auto t = q.front(); q.pop();
        if (st[t.first][t.second] || get_pair_sum(t) > threshold) continue;
        res++;
        st[t.first][t.second] = true;      // 标记已访问

        for (int i = 0; i < 4; i++) {      // 四个方向扩展
            int x = t.first + dx[i], y = t.second + dy[i];
            if (x >= 0 && x < rows && y >= 0 && y < cols)
                q.push({x, y});
        }
    }
    return res;
}
```

---

## 📝 总结

BFS 求可达区域数量，关键在约束条件：坐标数位和 <= k。由于行/列的各位数字独立，可用辅助函数提取数位并求和判断。

---

