# Dynamic Programming II

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

### Longest Common Substring 最长公共子序列 [nc](https://www.nowcoder.com/questionTerminal/02e7cc263f8a49e8b1e1dc9c116f7602)

---

#### Description

```
Given two strings ‘X’ and ‘Y’, find the length of the longest common substring.
```

#### Example:

```
"abcdxyz", "xyzabcd" => 4;
"zxabcdezy", "yzabcdezx" => 6
"1AB2345CD", "12345EF" => 4
```

#### Idea:

```
1. use the length of a and b to build a 2D array
row = a.length, col = b.length, init the value with 0
2. use i (index for a iteration) and j (index for b iteration) to check the letter matching
3. if a[i] === b[j], then check the diagonal value(a[i - 1][j - 1]); and set the current value to a[i - 1][j - 1] + 1
```

#### Base case:

```
dp = new Array(a.length).fill(new Array(b.length).fill(0))
```

#### Inductive step:

```
dp[i][j] = 1, i = 0 || j = 0
dp[i][j] = dp[i - 1][j - 1] + 1, i > 0 && j > 0
```

#### Edge Case:

```
"GeeksforGeeks", "GeeksQuiz" => 5
```

#### Code:

```JavaScript
function longestCommonSubstring(a, b) {
  /** Build the 2D array. */
  let dp = [];
  for (let i = 0; i < a.length; i++) {
    const row = [];
    for (let j = 0; j < b.length; j++) {
      row.push(0);
    }
    dp.push(row);
  }

  /** Records the answer. */
  let res = 0;

  /** Finds the matching letter between 2 strings. */
  for (let i = 0; i < a.length; i++) {
    for (let j = 0; j < b.length; j++) {
      if (a[i] === b[j]) {
        /**
         * Increases the count based on diagonal.
         * And handles the edge case.
         */
        if (i > 0 && j > 0)
          dp[i][j] = dp[i - 1][j - 1] + 1;
        else
          dp[i][j] = 1;
        res = Math.max(res, dp[i][j]);
      }
    }
  }
  return res;
}
```

### Least string conversion edit [nc](https://www.nowcoder.com/questionTerminal/04f1731f32e246b4a19688972d5e2600)

---

#### Description

```
对于两个字符串A和B，我们需要进行插入、删除和修改操作将A串变为B串，定义c0，c1，c2分别为三种操作的代价，请设计一个高效算法，求出将A串变为B串所需要的最少代价。

给定两个字符串A和B，及它们的长度和三种操作代价，请返回将A串变为B串所需要的最小代价。保证两串长度均小于等于300，且三种代价值均小于等于100。
```

#### Example:

```
"abc","adc", 5,3,100 => 8
```

#### Idea:

```
1. build a 2-D array(a + 1, b + 1) to record each minimum step
row -> a, col -> b
2. Set the payoff for `del` in each row head [i][0], and set the payoff for `add` in each col head [0][j]:
[
  [0, 5, 10, 15]
  [3,  ,  ,  ]
  [6,  ,  ,  ]
  [9,  ,  ,  ]
]
3. check dp[i - 1][j - 1]:
  case 1: if a[i - 1] == b[j - 1], keep the value dp[i][j] = value
  case 2: if a[i - 1] != b[j - 1], current b can be obtained by operating a `del` on the last a (dp[i][j - 1]) => dp[i][j - 1] + del
  case 3: if a[i - 1] != b[j - 1], current b can be obtained by operating a `add` on the last b (dp[i - 1][j]) => dp[i - 1][j] + add
  case 4: if a[i - 1] != b[j - 1], current b can be obtained by a simple edit from dp[i - 1][j - 1]

column上的都是要得到的
在上要减
在下要加
[
  [0, 5, 10, 15],
  [3, 0, 5, 10],
  [6, 3, 8, 13],
  [9, 6, 11, 8]
]
last col
row cur

cur = last + edit
cur = col + add
cur = row + del
```

#### Base case:

```
[
  [0, 5, 10, 15],
  [3, 0, 5, 10],
  [6, 3, 8, 13],
  [9, 6, 11, 8]
]

[
  [0, add, add, add],
  [del,  ,  ,  ,  ],
  [del,  ,  ,  ,  ],
  [del,  ,  ,  ,  ]
]

右移 => dp[i][j - 1] + add
下移 => dp[i - 1][j] + del
```

#### Inductive step:

```
f(edit) = f(x - 1, y - 1) + edit
f(add) = f(x, y - 1) + add
f(del) = f(x - 1, y) + del
f(x,y) = min(f(edit), f(add), f(del))
```

#### Code:

```JavaScript
function findMinCost(a, b, add, del, edit) {
  /** Build the 2D array. */
  let dp = [];
  for (let i = 0; i <= a.length; i++) {
    let row = [];
    for (let j = 0; j <= b.length; j++) {
      row.push(0);
    }
    dp.push(row);
  }

  /** Sets the payoff on each row head. */
  for (let i = 1; i < dp.length; i++)
    dp[i][0] = i * del;

  /** Sets the payoff on each col head. */
  for (let j = 1; j < dp[0].length; j++)
    dp[0][j] = j * add;

  for (let i = 1; i < dp.length; i++) {
    for (let j = 1; j < dp[i].length; j++) {
      /**
       * If the previous letters are the same, then inherit it.
       * Otherwise, compare the edit/del/add source.
       */
      if (a[i - 1] === b[j - 1])
        dp[i][j] = dp[i - 1][j - 1];
      else {
        const editCost = dp[i - 1][j - 1] + edit;
        const addCost = dp[i - 1][j] + del;
        const delCost = dp[i][j - 1] + add;
        dp[i][j] = Math.min(editCost, addCost, delCost);
      }
    }
  }
  return dp[a.length][b.length];
}
```

### Unique Paths [lc-62](https://leetcode.com/problems/unique-paths/)

---

#### Description

```
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?
```

![Alt text](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

#### Example:

```
Input: m = 3, n = 2
Output: 3
Input: m = 7, n = 3
Output: 28
```

#### Idea:

```
1. get the possible ways to render the edge
2. build the possible paths on each block: cur = left + up
```

#### Base case:

```
[
  [1, 1, 1, 1, 1]
  [1,  ,  ,  ,  ]
  [1,  ,  ,  ,  ]
  [1,  ,  ,  ,  ]
]
Denotes the possible ways to go through each edge
```

#### Inductive step:

```
cur = left + up
f(x, y) = f(x, y - 1) + f(x - 1, y)
```

#### Code:

```JavaScript
function uniquePaths(m, n) {
  const dp = [];
  for (let i = 0; i < m; i++) {
    const row = [];
    for (let j = 0; j < n; j++) {
      row.push(1);
    }
    dp.push(row);
  }

  for (let i = 1; i < dp.length; i++) {
    for (let j = 1; j < dp[i].length; j++) {
      dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
    }
  }

  return dp[m - 1][n - 1]
}
```

### Unique Paths II [lc-63](https://leetcode.com/problems/unique-paths-ii/)

---

#### Description

```
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?
```

![Alt text](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

#### Example:

```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
```

#### Idea:

```
1. Fix the edge (row heads and column heads), if there is a obstacle, all the following blocks will be blocked.
2. If find any obstacle from the grid, mark it as 0, means there is no possible way to get from the grid.
3. If no obstacle, then merge the up paths and left paths: cur = left + up.
```

#### Base case:

```
[
  [1, 1, 0, 0, 0]
  [0,  ,  ,  ,  ]
  [0,  ,  ,  ,  ]
  [0,  ,  ,  ,  ]
]
0 is obstacle
1 is possible path ways
```

#### Inductive step:

```
f(x, y) = f(x, y - 1) + f(x - 1, y)
```

#### Edge Case:

```
Input:
[
  [1, 0]
]
Output: 0
```

#### Code:

```JavaScript
function uniquePathsWithObstacles(grid) {
  grid[0][0] = grid[0][0] === 1 ? 0 : 1;
  /** Row head. */
  for (let i = 1; i < grid.length; i++) {
    grid[i][0] = grid[i][0] === 1 ? 0 : grid[i - 1][0];
  }

  for (let j = 1; j < grid[0].length; j++) {
    grid[0][j] = grid[0][j] === 1 ? 0 : grid[0][j - 1];
  }

  for (let i = 1; i < grid.length; i++) {
    for (let j = 1; j < grid[i].length; j++) {
      if (grid[i][j] === 1) {
        grid[i][j] = 0;
      } else {
        grid[i][j] = grid[i][j - 1] + grid[i - 1][j];
      }
    }
  }
  return grid[grid.length - 1][grid[0].length - 1];
}
```
