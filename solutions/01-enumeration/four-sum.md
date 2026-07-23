# 四数之和

> **难度**：困难 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：枚举算法

---

## 📝 题目描述

给定一个目标值 `target`，在整数数组 `A` 中找出四个元素 `(a, b, c, d)` 使 `a + b + c + d == target`。找出所有满足条件的四元组，按从小到大顺序输出，要求 `a < b < c < d` 且不允许重复数字。

**输入**：第一行有两个整数 `target` 和 `n`。第二行为数组 `A` 的 `n` 个元素，用空格隔开。

**输出**：所有满足条件的四元组，按 `a` 升序，`a` 相同按 `b` 升序，再按 `c` 升序。

**输入样例**：
```
17 7
0 2 5 10 15 18 25
```

**输出样例**：
```
0 2 5 10
```

---

## 💡 思路

在三数之和的基础上加一层循环：排序去重后，先固定前两个数 `a`、`b`，然后在剩余区间内用双指针 `c`（左）和 `d`（右）枚举后两个数，四数偏大时右指针左移收缩范围。

---

## 🔑 关键代码

```cpp
sort(nums.begin(), nums.end());                         // 升序排序
nums.erase(unique(nums.begin(), nums.end()), nums.end()); // 去重

for (int i = 0; i < nums.size(); i++)                   // 固定第一个数
    for (int j = i + 1; j < nums.size(); j++) {         // 固定第二个数
        for (int k = j + 1, l = nums.size() - 1; k < l; k++) {  // k 左指针，l 右指针
            while (k < l - 1 && nums[i] + nums[j] + nums[k] + nums[l - 1] >= target) l--;
            if (nums[i] + nums[j] + nums[k] + nums[l] == target)
                res.push_back({nums[i], nums[j], nums[k], nums[l]});
        }
    }
```

---

## 📝 总结

`k` 数之和问题的通用框架：外层固定前 `k-2` 个值，最内层用双指针扫描。排序保证输出有序，去重避免重复结果。

---

