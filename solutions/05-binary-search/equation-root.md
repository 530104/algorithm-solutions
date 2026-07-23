# 求方程的根

> **难度**：简单 &nbsp;&nbsp;|&nbsp;&nbsp; **分类**：二分算法

---

## 📝 题目描述

求方程 x^3 - 5x^2 + 10x - 80 = 0 的根，精确到小数点后 9 位。

**输出**：`5.705085930`

---

## 💡 思路

在单调递增区间内用浮点数二分：若 `f(mid) > 0`，根在左侧 `r = mid`；否则根在右侧 `l = mid`。控制精度 `r - l > 1e-11` 保证小数点后 9 位准确。

---

## 🔑 关键代码

```cpp
double solve(double l, double r) {
    double eps = 1e-11;                          // 精度要求：比输出多两位
    while (r - l > eps) {                        // 区间长度大于精度就继续二分
        double mid = (l + r) / 2;
        // 计算 f(mid) = mid^3 - 5*mid^2 + 10*mid - 80
        if (mid * mid * mid - 5 * mid * mid + 10 * mid - 80 > 0)
            r = mid;                             // f>0，根在左侧
        else
            l = mid;                             // f<=0，根在右侧
    }
    return l;                                    // l 和 r 已足够接近，返回任意一个
}
// printf("%.9lf\n", solve(0.0, 10.0));
```

---

## 📝 总结

浮点数二分无需处理边界，直接 `l = mid` 或 `r = mid`。精度控制取 `1e-(输出位数+2)` 即可保证结果准确。

---

