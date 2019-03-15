# Bit Manipulation

```
Solves:
1. 运算速度技巧
2. 装逼
```

### 乘除

`k >> n` 指将 k 除 2 的 n 次方
<br>
`k << n` 指将 k 撑 2 的 n 次方

### 并或域

`&` 指并域(可用于找奇偶树)

```JavaScript
(2n + 1) & 1等于1
(2n) & 1等于0

求运算
a = a % 8(a必须为2的n次方)

可以改为:
a = a & 7

操作规则
1 & 1 = 1
1 & 0 = 0
0 & 1 = 0
0 & 0 = 0
...
k & -1(全1) = k
k & k = k
0 & k = 0
k & ~k = 0
```

`|` 指或域(可用于确定或的关系)

```JavaScript
a |= false 会得到1
2.333 | 0 = 2 可用于去尾数
```

`^` 指异域

```JavaScript
操作规则
0^0=0
0^1=1
1^0=1
1^1=0
...
k ^ k = 0 = k & ~k
0 ^ k = k = k & k
```

#### 例子

```Bash
13 = 00001101
20 = 00010100
25 = 00011001 = 13 ^ 20
```

#### 作用

`消除相同的数` 或 `抵消出现偶数次的数` 或 `找出只出现过一次的数`

```
2 ^ 2 = 0
3 ^ 3 = 0
...
k ^ k = 0
...
1 ^ 2 ^ 2 = 1
...
0 ^ k = k
```

### QUESTION: Single Number [lc-136](https://leetcode.com/problems/single-number/description/)

---

#### DESCRIPTION

```
Given a non-empty array of integers, every element appears twice except for one.
Find that single one.
Implement it without using extra memory
```

#### EXAMPLE

```
Input: [2,2,1]
Output: 1
Input: [4,1,2,1,2]
Output: 4
```

#### Code:

```JavaScript
function singleNumber(nums) {
  return nums.reduce((a, b) => a ^ b);
}
```

`~` 指反域

```JavaScript
~0 = -1
~1 = -2
~k = -k - 1
...
~-1 = 0
~-2 = 1
```

### Single Number II [lc-137](https://leetcode.com/problems/single-number-ii/)

---

#### Description

```
Given a non-empty array of integers,
every element appears three times except for one,
which appears exactly once. Find that single one.
Implement it without using extra memory
```

#### Example:

```
Input: [2,2,3,2]
Output: 3
Input: [0,1,0,1,0,1,99]
Output: 99
```

#### Idea:

```
00 -> 0k -> k0 -> 00
态三 -> 态一 -> 态二 -> 态三

态一:
一开始, low(即'0') ^ num = num
num(以上) & ~high(即'0') = num
最终low得'num'

high(即'0') ^ num = num
num(以上) & ~low(即'num') = 0(被抵消)
最终high得'0' => 由此进入态二


态二:
low(即'num') ^ num(假设与态一相同) = 0
0 & ~high(即'0') = 0(抵消)
最终low得'0'

high(即'0') ^ num = num
num & ~low(即'0') = num(high继承low)
最终high得'num'  => 由此进入态三


态三:
low(即'0') ^ num(假设与态一相同) = num
num & ~high(即'num') = 0(抵消)
最终low得'0'

high(即'num') ^ num = 0
0 & ~low(即'0') = 0(抵消)
最终high得'0' => 完全回归初环境
```

#### Code:

```JavaScript
function singleNumber(nums) {
  let [low, high] = [0, 0];
  for (const num of nums) {
    low ^= num & ~high;
    high ^= num & ~low;
  }
  return low;
}
```
