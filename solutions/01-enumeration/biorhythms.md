# 人的周期

> **难度**：简单 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：枚举算法

---

## 📝 题目描述

人的体力周期 23 天、感情周期 28 天、智力周期 33 天。每个周期有一天是高峰。给定三个周期在当年第一次出现高峰的天数（从当年第一天算起），以及一个起始日期 $d$，求下一次三个高峰同时出现的日期距离 $d$ 的天数（不包括 $d$ 当天）。

## 输入

多组数据，每组四个整数 $p, e, i, d$。以 `-1 -1 -1 -1` 结束。所有时间 $<= 365$，答案 $<= 21252$。

## 输出

```
Case 1: the next triple peak occurs in X days.
```

**输入样例**：
```
0 0 0 0
0 0 0 100
5 20 34 325
4 5 6 7
283 102 23 320
203 301 203 40
-1 -1 -1 -1
```

**输出样例**：
```
Case 1: the next triple peak occurs in 21252 days.
Case 2: the next triple peak occurs in 21152 days.
Case 3: the next triple peak occurs in 19575 days.
Case 4: the next triple peak occurs in 16994 days.
Case 5: the next triple peak occurs in 8910 days.
Case 6: the next triple peak occurs in 10789 days.
```

---

## 💡 思路

从给定起始日期开始逐天向后枚举，找到体力、情感、智力三个周期同时达到峰值的那一天。

---

## 🔑 关键代码

```cpp
for (k = d + 1; k <= d + 21252; k++)       // 从 d+1 开始逐天枚举，答案不超过 21252 天
    if ((k - p) % 23 == 0 &&               // 体力周期 23 天
        (k - e) % 28 == 0 &&               // 感情周期 28 天
        (k - i) % 33 == 0)                 // 智力周期 33 天
        cout << "Case " << caseNumber
             << ": the next triple peak occurs in " << k - d << " days." << endl;
```

---

## 📝 总结

利用周期规律可以减少枚举次数，注意取模运算处理日期偏移。