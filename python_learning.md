# 数据类型
## 整数（int）
age = 25

## 小数（float）
height = 1.68

## 字符串（str）——文字必须放引号
name = "Xiaoming"

## 布尔值（bool）—— 只有 True 或 False
is_student = True

# Operator
% - 除余 - 7%3 -余1
** - 幂 - 2**3 - 8

# import math


| 函数              | 作用   | 示例                    | 说明               |
| --------------- | ---- | --------------------- | ---------------- |
| `math.ceil(x)`  | 向上取整 | `math.ceil(3.2)` → 4  | 最小的**大于等于 x**的整数 |
| `math.floor(x)` | 向下取整 | `math.floor(3.7)` → 3 | 最大的**小于等于 x**的整数 |
| `math.trunc(x)` | 截断整数 | `math.trunc(3.9)` → 3 | 去掉小数部分，不四舍五入     |
| `math.fabs(x)`  | 绝对值  | `math.fabs(-5)` → 5.0 | 返回 float 类型      |

| 函数                  | 作用         | 示例                        | 说明                       |
| ------------------- | ---------- | ------------------------- | ------------------------ |
| `math.pow(x, y)`    | 幂          | `math.pow(2, 3)` → 8.0    | 等同于 `x ** y`，但总是返回 float |
| `math.sqrt(x)`      | 平方根        | `math.sqrt(16)` → 4.0     |                          |
| `math.exp(x)`       | e 的 x 次方   | `math.exp(1)` → 2.718...  | e ≈ 2.718                |
| `math.log(x)`       | 自然对数 ln(x) | `math.log(10)` → 2.302... | 默认以 e 为底                 |
| `math.log(x, base)` | 指定底的对数     | `math.log(8, 2)` → 3.0    | log₂(8)                  |
| `math.log10(x)`     | 以 10 为底的对数 | `math.log10(100)` → 2.0   |                          |

| 名称         | 含义               | 示例         |
| ---------- | ---------------- | ---------- |
| `math.pi`  | 圆周率 π ≈ 3.14159  | `math.pi`  |
| `math.e`   | 自然常数 e ≈ 2.71828 | `math.e`   |
| `math.tau` | 圆周的两倍 τ = 2π     | `math.tau` |

| 函数                | 作用    | 示例                            |
| ----------------- | ----- | ----------------------------- |
| `math.sin(x)`     | 正弦    | `math.sin(math.pi/2)` → 1.0   |
| `math.cos(x)`     | 余弦    | `math.cos(0)` → 1.0           |
| `math.tan(x)`     | 正切    | `math.tan(math.pi/4)` → 1.0   |
| `math.asin(x)`    | 反正弦   | `math.asin(1)` → `π/2`        |
| `math.acos(x)`    | 反余弦   | `math.acos(1)` → 0            |
| `math.atan(x)`    | 反正切   | `math.atan(1)` → `π/4`        |
| `math.radians(x)` | 角度转弧度 | `math.radians(180)` → `π`     |
| `math.degrees(x)` | 弧度转角度 | `math.degrees(math.pi)` → 180 |

| 函数                    | 作用          | 示例                        | 说明          |
| --------------------- | ----------- | ------------------------- | ----------- |
| `math.factorial(n)`   | 阶乘          | `math.factorial(5)` → 120 | 只支持整数       |
| `math.gcd(a, b)`      | 最大公约数       | `math.gcd(12, 18)` → 6    |             |
| `math.lcm(a, b)`      | 最小公倍数       | `math.lcm(4, 6)` → 12     | Python 3.9+ |
| `math.comb(n, k)`     | 组合数 C(n, k) | `math.comb(5, 2)` → 10    | Python 3.8+ |
| `math.perm(n, k)`     | 排列数 A(n, k) | `math.perm(5, 2)` → 20    | Python 3.8+ |
| `math.prod(iterable)` | 所有元素连乘      | `math.prod([2,3,4])` → 24 | Python 3.8+ |


