# Dynamic Programming III

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

### Decode Ways [lc-91](https://leetcode.com/problems/decode-ways/)

---

#### Description

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits,
determine the total number of ways to decode it.
```

#### Example:

```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).

Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

#### Idea:

```
`oneDigitCount` to store the possible paths of adding current digit to the previous results.
`twoDigitCount` to store the possible paths of adding current and last digit to the previous results.
```

#### Base case:

```
twoDigitCount = 0;
oneDigitCount = 1;
```

#### Inductive step:

```
current count = oneDigitCount + twoDigitCount
```

#### Edge Case:

```
console.log(numDecodings('0') === 0);
console.log(numDecodings('10') === 1);
console.log(numDecodings('100') === 0);
```

#### Code:

```JavaScript
function numDecodings(s) {
  /** Possible paths that can add check digit on. */
  let oneDigit = 1;
  /** Possible paths that can add check and last digits on. */
  let twoDigit = 0;

  for (let i = 1; i <= s.length; i++) {
    const checkDigit = Number(s[i - 1]);
    const lastDigit = Number(s[i - 2]) || 0;
    /** Defaults the current digit count to 0. */
    let curDigit = 0;

    /** Check one digit. */
    curDigit += checkDigit === 0 ? 0 : oneDigit;

    /** Check two digit. */
    const num = lastDigit * 10 + checkDigit;
    curDigit += num >= 10 && num <= 26 ? twoDigit : 0;

    /** Rolling. */
    [twoDigit, oneDigit] = [oneDigit, curDigit];
  }
  return oneDigit;
}
```

### Decode Ways II [lc-639](https://leetcode.com/problems/decode-ways-ii/)

---

#### Description

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
Beyond that, now the encoded string can also contain the character '*', which can be treated as one of the numbers from 1 to 9.

Given the encoded message containing digits and the character '*', return the total number of ways to decode it.

Also, since the answer may be very large, you should return the output mod 109 + 7.
```

#### Example:

```
Input: "*"
Output: 9
Explanation: The encoded message can be decoded to the string: "A", "B", "C", "D", "E", "F", "G", "H", "I".

Input: "1*"
Output: 9 + 9 = 18
```

#### Idea:

```
1. Convert the checkDigit and lastDigit as arrays.
2. Use those array to create those combinations and check the num is in range.
```

#### Base case:

```JavaScript
/** Possible paths that can add check digit on. */
let oneDigit = 1;
/** Possible paths that can add check and last digits on. */
let twoDigit = 0;
```

#### Inductive step:

```JavaScript
[twoDigit, oneDigit] = [oneDigit, curDigit % (10 ** 9 + 7)]
```

#### Edge Case:

```JavaScript
console.log(numDecodings('*1') === 11);
console.log(numDecodings('11') === 2);
console.log(numDecodings('1*') === 18);
console.log(numDecodings('**') === 96);
```

#### Code:

```JavaScript
function numDecodings(s) {
  /** Possible paths that can add check digit on. */
  let oneDigit = 1;
  /** Possible paths that can add check and last digits on. */
  let twoDigit = 0;
  /** Template array from 1 to 9. */
  const counts = new Array(9).fill().map((v, i) => i + 1);

  for (let i = 1; i <= s.length; i++) {
    /** Converts the last digit and check digit into arrays. */
    const checkDigit = s[i - 1] === '*' ? counts.slice() : [Number(s[i - 1])];
    const lastDigit = s[i - 2] === '*' ? counts.slice() : [Number(s[i - 2]) || 0];
    let curDigit = addDigitPaths(lastDigit, checkDigit, oneDigit, twoDigit);

    /** Rolling. */
    [twoDigit, oneDigit] = [oneDigit, curDigit % (10 ** 9 + 7)]
  }
  return oneDigit;
}

function addDigitPaths(lastDigit, checkDigit, oneDigit, twoDigit) {
  let curDigit = 0;
  for (const check of checkDigit) {
      curDigit += check === 0 ? 0: oneDigit;
    for (const last of lastDigit) {
      const num = last * 10 + check;
      if (num >= 10 && num <= 26) {
        curDigit += twoDigit;
      }
    }
  }
  return curDigit;
}
```

### Frog Jump [lc-403](https://leetcode.com/problems/frog-jump/)

---

#### Description

```
Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was k units, then its next jump must be either k - 1, k, or k + 1 units. Note that the frog can only jump in the forward direction.
```

#### Example:

```
[0,1,3,5,6,8,12,17]

There are a total of 8 stones.
The first stone at the 0th unit, second stone at the 1st unit,
third stone at the 3rd unit, and so on...
The last stone at the 17th unit.

Return true. The frog can jump to the last stone by jumping
1 unit to the 2nd stone, then 2 units to the 3rd stone, then
2 units to the 4th stone, then 3 units to the 6th stone,
4 units to the 7th stone, and 5 units to the 8th stone.

[0,1,2,3,4,8,9,11]

Return false. There is no way to jump to the last stone as
the gap between the 5th and 6th stone is too large.
```

#### Idea:

```
1. use a map to mark all (stoneValue, index) pairs for searching stone index purpose later

2. build a 2D board to mark `the path can be obtains`(横, with index j) and `the original array`(竖, with index i)

3. iterate from (1 -> j, (1 -> i)), mark all the possible paths top-down & left -> right

4. search for last possible paths in the transversal
- arr[j] - i => last check values
- map.get(lastCheckValue) => lastCheckValueIndex => get the tracking => `j``
- mark dp[i][j] = dp[i - 1][j`] ||
dp[i][j`] || dp[i + 1][j`]
(-1, 0, +1换化成了check之前一列的上中下)
- check the final column on the board, if there is any possible path, then return true, otherwise, return false.
```

#### Base case:

```
dp[0][0] = true
Because the first step is guarantee
```

#### Inductive step:

```
dp[i][j] = dp[i - 1][lastStoneIndex]
|| dp[i][lastStoneIndex]
|| dp[i + 1][lastStoneIndex]
```

#### Edge Case:

```
[0, 2] => false
```

#### Code:

```JavaScript
function canCross(arr) {
  /** Build a board to store the existence of solution. */
  const dp = arr.map(_ => arr.map(_ => 0));
  dp[0][0] = 1;

  /** Build a map to store the value-index pairs. */
  const map = new Map(arr.map((stone, i) => [stone, i]));

  for (let j = 1; j < dp[0].length; j++) {
    for (let i = 1; i < dp.length; i++) {
      const lastStoneValue = arr[j] - i;
      if (map.has(lastStoneValue)) {
        /** Check the last touchable stones' existence. */
        const lastStoneIndex = map.get(lastStoneValue);
        /** check [-1, 0, +1] */
        dp[i][j] = dp[i - 1][lastStoneIndex] || dp[i][lastStoneIndex]
        if (i < dp.length - 1)
          dp[i][j] |= dp[i + 1][lastStoneIndex];
      }
    }
  }

  /** Check each ending block of the row. */
  return dp.some(row => row.pop());
}
```

#### Idea2:

```
index:        0   1   2   3   4   5   6   7
            +---+---+---+---+---+---+---+---+
stone pos:  | 0 | 1 | 3 | 5 | 6 | 8 | 12| 17|
            +---+---+---+---+---+---+---+---+
k:          | 1 | 0 | 1 | 1 | 0 | 1 | 3 | 4 |
            |   | 1 | 2 | 2 | 1 | 2 | 4 | 5 |
            |   | 2 | 3 | 3 | 2 | 3 | 5 | 6 |
            |   |   |   |   | 3 | 4 |   |   |
            |   |   |   |   | 4 |   |   |   |
            |   |   |   |   |   |   |   |   |

Convert to the dp 2D-board:
top(j) -> k
left(i) -> stones & index

[   0  1  2  3  4  5  6  7
0 [ 0, 1, 0, 0, 0, 0, 0, 0 ],
1 [ 1, 1, 1, 0, 0, 0, 0, 0 ],
2 [ 0, 1, 1, 1, 0, 0, 0, 0 ],
3 [ 0, 1, 1, 1, 0, 0, 0, 0 ],
4 [ 1, 1, 1, 1, 1, 0, 0, 0 ],
5 [ 0, 1, 1, 1, 1, 0, 0, 0 ],
6 [ 0, 0, 0, 1, 1, 1, 0, 0 ],
7 [ 0, 0, 0, 0, 1, 1, 1, 0 ] ]
```

#### Base case:

```
[ [ 0, 1, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 0, 0, 0, 0, 0 ] ]
```

#### Inductive step:

```
if dp[j][gap]:
  dp[i][gap - 1] = 1
  dp[i][gap] = 1
  dp[i][gap + 1]
```

#### Code:

```JavaScript
function canCross(stones) {
  /** Build a board to store the existence of solution. */
  const n = stones.length;
  const dp = stones.map(_ => stones.map(_ => 0));
  dp[0][1] = 1;

  for (let i = 1; i < n; i++) {
    for (let j = 0; j < i; j++) {
      /** The gap between the current block to the previous */
      const gap = stones[i] - stones[j];

      /**
       * stones[i] must larger than stones[j] in ascending order.
       * hasPossibleJump denotes the possible jump to fill out the gap
       * on arr[j] stone.
       * hasPossibleJump is used to locate
       * the last possible jumping stone => stones[j]
       * and the step it takes to jump dp[j][gap]
       */
      const hasPossibleJump = dp[j][gap];
      if (!hasPossibleJump) continue;
      /** Check if exist possible path to get into the last stone. */
      if (i === n - 1) return true;

      /** dp[i][gap] denotes the last jump step count. */
      dp[i][gap - 1] = gap > 0 ? 1 : 0;
      dp[i][gap] = 1;
      dp[i][gap + 1] = gap < n ? 1 : 0;
    }
  }
  return false;
}
```
