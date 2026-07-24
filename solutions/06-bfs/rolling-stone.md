# 滚石柱

> **难度**：困难 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：广度优先搜索（BFS）

---

## 📝 题目描述

一个 1x1x2 的长方体石柱在 N x M 的迷宫中滚动。石柱有 3 种放置形式：立（1x1 触地）、横躺、竖躺。每次按键向上下左右方向沿棱滚动 90 度。不能接触禁地 `#`，不能立在易碎地 `E` 上。求滚动到目标 `O`（立在上面）的最少步数。N, M <= 500。

地图字符：`.` 硬地，`E` 易碎地，`#` 禁地，`X` 起点，`O` 终点。

**输出**：最少步数，无解输出 `Impossible`。

**输入样例**：
```
7 7
#######
#..X###
#..##O#
#....E#
#....E#
#.....#
#######
0 0
```

---

## 💡 思路

三维 BFS：状态 `(x, y, z)`，z=0 立，z=1 横躺（占左右两格），z=2 竖躺（占上下两格）。根据当前姿态和移动方向，用预计算的转移表 `dxyz` 确定新坐标和新姿态。`isvalid()` 根据姿态额外检查占据的格子是否合法。

---

## 🔑 关键代码

```cpp
struct Stone { int x, y, z; };                     // (x,y)左上角坐标, z姿态

// 转移表：dxyz[方向][当前姿态] = {dx, dy, 新姿态}
// 方向: 0=上, 1=下, 2=左, 3=右
int dxyz[4][3][3] = {
    {{-2,0,2}, {-1,0,1}, {-1,0,0}},                // 向上滚
    {{ 1,0,2}, { 1,0,1}, { 2,0,0}},                // 向下滚
    {{ 0,-2,1},{ 0,-1,0},{ 0,-1,2}},               // 向左滚
    {{ 0, 1,1},{ 0, 2,0},{ 0, 1,2}}                // 向右滚
};

bool isvalid(Stone t) {
    if (!isinside(t.x, t.y) || area[t.x][t.y] == '#') return false;
    if (t.z == 2 && (!isinside(t.x+1, t.y) || area[t.x+1][t.y] == '#'))
        return false;                               // 竖躺：多占下方一格
    if (t.z == 1 && (!isinside(t.x, t.y+1) || area[t.x][t.y+1] == '#'))
        return false;                               // 横躺：多占右方一格
    if (t.z == 0 && area[t.x][t.y] == 'E')
        return false;                               // 立着不能站在易碎地面上
    return true;
}

int bfs(Stone s) {
    memset(dist, -1, sizeof(dist));
    queue<Stone> q;
    q.push(s);
    dist[s.x][s.y][s.z] = 0;
    while (q.size()) {
        Stone t = q.front(); q.pop();
        for (int i = 0; i < 4; i++) {               // 四个方向滚动
            Stone p = movestone(t, i);
            if (!isvalid(p) || dist[p.x][p.y][p.z] != -1) continue;
            dist[p.x][p.y][p.z] = dist[t.x][t.y][t.z] + 1;
            q.push(p);
            if (p.x == target.x && p.y == target.y && p.z == target.z)
                return dist[p.x][p.y][p.z];         // 到达目标
        }
    }
    return -1;
}
```

---

## 📝 总结

三维状态 BFS 的经典题。难点在于用转移表建模石柱的 3 种姿态 x 4 个方向的滚动结果，以及 `isvalid()` 中根据姿态检查不同的占地区域。预处理转移表后 BFS 部分与普通 BFS 一致。

---

