# 没有上司的舞会

> **难度**：中等 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：动态规划

---

## 📝 题目描述

N 名职员，关系是一棵以校长为根的树，父节点是子节点的直接上司。每人有快乐指数 Hi。邀请部分职员参会，不能同时邀请直接上下级，求最大快乐指数总和。N <= 6010。

**输入**：第一行 N，接下来 N 行 Hi，接下来 N-1 行每行 L K（K 是 L 的直接上司）。

**输出**：最大快乐指数。

**输入样例**：
```
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
```

**输出样例**：
```
5
```

---

## 💡 思路

树形 DP：`f[u][0]` = 不选 u 时子树最大快乐，`f[u][1]` = 选 u 时子树最大快乐。转移：`f[u][0] += max(f[v][0], f[v][1])`（u 不选，子可随便），`f[u][1] += f[v][0]`（u 选，子必不选）。DFS 自底向上递推。

---

## 🔑 关键代码

```cpp
const int N = 6010;
int n, happy[N];
int h[N], e[N], ne[N], idx;        // 邻接表存树
int f[N][2];                       // f[u][0]: 不选u, f[u][1]: 选u
bool has_fa[N];                    // 找根节点

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u) {
    f[u][1] = happy[u];            // 选 u，先加上 u 的快乐值
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];              // 子节点
        dfs(v);                    // 先递归处理子树
        f[u][1] += f[v][0];        // 选 u，子必不选
        f[u][0] += max(f[v][0], f[v][1]); // 不选 u，子可选可不选
    }
}

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) scanf("%d", &happy[i]);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i++) {
        int a, b;
        scanf("%d%d", &a, &b);     // b是a的上司
        add(b, a);
        has_fa[a] = true;
    }
    int root = 1;
    while (has_fa[root]) root++;   // 找树根（没有上司的节点）

    dfs(root);
    printf("%d\n", max(f[root][0], f[root][1])); // 根可选可不选
    return 0;
}
```

---

## 📝 总结

树形 DP 经典题。每个节点维护"选/不选"两种状态，子节点状态决定父节点转移。DFS 保证子节点先于父节点计算，`has_fa` 数组用于找出根节点。

---

