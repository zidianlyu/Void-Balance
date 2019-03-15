# Dynamic Programming I

#### Shift + Cmd + v for preview

```
Solves:
1. 找路径存在性可能
2. 总和最优
3. 最短路径
4. 最长公共子路径

模版
1. 最优子问题
2. 转移状态方程
```

#### [lc-DP](https://leetcode.com/tag/dynamic-programming/)

### Coin Change [lc-322](https://leetcode.com/problems/coin-change/)

---

#### Write a function to compute the fewest number of coins that you need to make up that amount.

#### If that amount of money cannot be made up by any combination of the coins, return `-1`.

```
Example:
Input: coins = [1, 2, 5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

#### Idea:

```
Uses an array to record the minimum of each step
```

#### Base case:

```
arr[1] = 1
arr[2] = arr[1] + 1 = 2
arr[3] = 1
arr[4] = arr[3] + 1 = 2
arr[5] = 1
...
```

#### Inductive step:

```
Finds the minimum possible from the previous steps.
f(x) = min(f(x - 1), f(x - 3), f(x - 5)) + 1, for x > 5
```

#### Edge Case:

```
[2], 3 => -1
[1, 2, 5], 11 => 3
```

#### Code:

```JavaScript
function coinChange(coins, amount) {
  /** The maximum possible value can not be greater than amount. */
  const maxValue = amount + 1;
  let arr = new Array(amount + 1).fill(maxValue);
  /**
   * Init the first element as 0,
   * therefore the 1st element can use it as a base.
   */
  arr[0] = 0;
  for (let i = 1; i <= amount; i++) {
    for (const coin of coins) {
      /** Avoid index out of bound. */
      if (coin <= i)
        arr[i] = Math.min(arr[i], arr[i - coin] + 1);
    }
  }
  /**
   * If the min operation is fail,
   * then the number on that possible would not change.
   */
  return arr[amount] === maxValue ? -1 : arr[amount];
}
```

### Minimum Coin Change Path (变形)

---

#### Code:

```JavaScript
function coinChange(coins, amount) {
  /** The maximum possible value can not be greater than amount. */
  const maxValue = amount + 1;
  let arr = new Array(amount + 1).fill(maxValue);
  /**
   * Init the first element as 0,
   * therefore the 1st element can use it as a base.
   */
  arr[0] = 0;
  const selectedCoins = [];
  for (let i = 1; i <= amount; i++) {
    let selectedCoin = maxValue;
    for (const coin of coins) {
      /** Avoid index out of bound. */
      if (coin <= i && arr[i - coin] + 1 < arr[i]) {
        arr[i] = arr[i - coin] + 1;
        selectedCoin = coin;
      }
    }
    selectedCoins.push(selectedCoin)
  }

  /**
   * If the min operation is fail,
   * then the number on that possible would not change.
   */
  if (arr[amount] === maxValue) return [];

  const res = [];
  let i = selectedCoins.length - 1;
  while (amount > 0) {
    res.push(selectedCoins[i]);
    /** locates the next target base. */
    i -= selectedCoins[i];
    /** Updates the remaining amount. */
    amount -= selectedCoins[i];
  }
  return res;
}
```

### Triangle 三角形求最小路径 [(lc-120)](https://leetcode.com/problems/triangle/)

---

#### Given a triangle, find the minimum path sum from top to bottom.

```
Example:
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
=> [2, 3, 5, 1]
```

#### Idea:

```
Bottom(right) up transverse
当前 = 下 + 右下
```

#### Base Case:

```
The original triangle array input.
```

#### Inductive Step:

```
The state transfer function:
<br>
**dp[i][j] += min(dp[i + 1], dp[i + 1][j + 1])**
```

#### Code:

```JavaScript
function minimumTotal(triangle) {
  for (let i = triangle.length - 2; i >= 0; i--) {
    for (let j = triangle[i].length - 1; j >= 0; j--) {
      triangle[i][j] += Math.min(triangle[i + 1][j], triangle[i + 1][j + 1]);
    }
  }
  return triangle[0][0];
}
```

### Longest Increasing Subsequence 最长递增子序列 [lc-300](https://leetcode.com/problems/longest-increasing-subsequence/)

---

#### Given an unsorted array of integers, find the length of longest increasing subsequence.

```
Input: [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

#### Idea:

```
Iterates through the nums array. (i, from 1)
Iterates all sub-arrays based on the current index. (j)
Finds the number that is smaller than the current (i) one,
if arr[j] < arr[i], dp[i] = dp[j] + 1
In dp level, the second for-loop counts the numbers that are that are smaller than the current one. And we use the dp[j] to avoid duplicate calculation.
```

#### Base case:

```
dp = new Array(nums.length + 1).fill(1)
or dp[0] = 1
```

#### Inductive step:

```
f(x) = max(f(x), f(y) + 1)
```

#### Edge Case:

```
lengthOfLIS([]) === 0
lengthOfLIS([0]) === 1
```

#### Code:

```JavaScript
function lengthOfLIS(nums) {
  if (!nums.length) return 0;
  let dp = new Array(nums.length + 1).fill(1);
  let res = 1;
  for (let i = 1; i < nums.length; i++) {
    for (let j = 0; j < i; j++) {
      if (nums[j] < nums[i]) {
        dp[i] = Math.max(dp[i], dp[j] + 1);
        res = Math.max(res, dp[i])
      }
    }
  }
  return res;
}
```
