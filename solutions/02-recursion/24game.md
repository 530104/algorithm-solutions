# 算 24

> **难度**：简单 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：递归算法

---

## 📝 题目描述

给出 4 个小于 10 的非负整数，使用加减乘除 4 种运算以及括号，判断能否算出 24。数字次序可以改变，除法为实数除法。

**输入**：多行，每行 4 个整数。最后一行为 4 个 0 表示结束，不处理。

**输出**：每行输出 `YES` 或 `NO`。

**输入样例**：
```
5 5 5 1
1 1 4 2
0 0 0 0
```

**输出样例**：
```
YES
NO
```

---

## 💡 思路

递归缩小问题规模：从 $n$ 个数中任选两个，进行加减乘除四种运算之一，将结果放回数组替换原两数，递归处理剩下的 $n-1$ 个数。$n=1$ 时检验结果是否等于 24。运算符选择时注意/需检查除数不为 0。

---

## 🔑 关键代码

```cpp
bool dfs(vector<double> &nums) {
    if (nums.size() == 1) return fabs(nums[0] - 24) < 1e-6;  // 只剩一个数，判断
    for (int i = 0; i < nums.size(); i++)
        for (int j = 0; j < nums.size(); j++) {
            if (i == j) continue;                              // 不能选同一个数
            vector<double> next;
            for (int k = 0; k < nums.size(); k++)
                if (k != i && k != j) next.push_back(nums[k]); // 保留其余数
            for (int op = 0; op < 4; op++) {                   // 四种运算
                if (op == 0) next.push_back(nums[i] + nums[j]);
                if (op == 1) next.push_back(nums[i] - nums[j]);
                if (op == 2) next.push_back(nums[i] * nums[j]);
                if (op == 3 && nums[j] != 0) next.push_back(nums[i] / nums[j]);
                if (dfs(next)) return true;                    // 递归验证
                next.pop_back();                               // 回溯
            }
        }
    return false;
}
```

---

## 📝 总结

两两组合 + 四种运算的递归搜索，用浮点数处理实数除法。枚举所有数字对和所有运算，等价于穷举所有可能的括号组合。

---

