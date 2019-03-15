# Dynamic Programming IV

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

### Word Break [lc-139](https://leetcode.com/problems/word-break/)

---

#### Description

```
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words。
Determine if s can be segmented into a space-separated sequence of one or more dictionary words.
```

#### Example:

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

#### Idea:

```
1. find the start point to match the string
2. get the substring of the (start, cur), check the word match on dict.

```

#### Base case:

```
dp = [s.length + 1]
dp[0] = true (also means empty string)
```

#### Inductive step:

```
dp[i] = true if (dp[j] = true && s.substring(j, i) in dict), 0 < j < i
```

#### Code:

```JavaScript
function wordBreak(s, dict) {
  /** 留多一个位置,默认开始为true(empty string) */
  const dp = [true];
  for (let i = 1; i <= s.length; i++) dp.push(false);

  for (let i = 1; i <= s.length; i++) {
    for (let j = 0; j < i; j++) {
      /**
       * 第一个从0算起
       * 1. 一方面要和开头对上
       * 2. 另一方面,要标识 valid的开头到cur,是否在dict里面
       */
      if (dp[j] && dict.includes(s.substring(j, i))) {
        dp[i] = true;
        break;
      }
    }
  }
  return dp[s.length];
}
```

### Word Break II [lc-140](https://leetcode.com/problems/word-break-ii/)

---

#### Description

```
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.
```

#### Example:

```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

#### Idea:

```
1. Use work break I to check the edge case.
2. Mark string on each block.
```

#### Base case:

```JavaScript
[[''], [], [], []...]
```

#### Inductive step:

```JavaScript
if (dp[j] && dict.includes(word)) {
  for (const subString of dp[j]) {
    dp[i].push(`${subString} ${word}`.trim());
  }
}
```

#### Edge Case:

```
const s = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
const dict = ["a", "aa", "aaa", "aaaa", "aaaaa", "aaaaaa", "aaaaaaa", "aaaaaaaa", "aaaaaaaaa", "aaaaaaaaaa"]
```

#### Code:

```JavaScript
function wordBreak(s, dict) {
  /**
   * Basically, here is work break I,
   * used it to kill the case 'aaaabaaa...'
   */
  const check = [true];
  for (let i = 1; i <= s.length; i++) check.push(false);

  for (let i = 1; i <= s.length; i++) {
    for (let j = 0; j < i; j++) {
      const word = s.substring(j, i);
      if (check[j] && dict.includes(word)) {
        check[i] = true;
        /**
         * If there is one possible solution, then we can set it to true.
         * We do not need to check all the possible cases.
         */
        break;
      }
    }
  }
  if (!check[s.length]) return [];

  /** Initialize the start with a empty string for checking purpose. */
  const dp = [['']];
  for (let i = 1; i <= s.length; i++) dp.push([]);

  for (let i = 1; i <= s.length; i++) {
    dp.push([]);
    for (let j = 0; j < i; j++) {
      const word = s.substring(j, i);
      if (dp[j] && dict.includes(word)) {
        for (const subString of dp[j]) {
          dp[i].push(`${subString} ${word}`.trim());
        }
      }
    }
  }
  return dp[s.length];
}
```
