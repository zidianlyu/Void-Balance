# Two Pointers II

### QUESTION: Min Windown [lc-76](https://leetcode.com/problems/minimum-window-substring/)

---

#### DESCRIPTION

```JavaScript
Count the minimum substring from the first input string that contains all the letter from the second input string
For example:
('ADOBECODEBANC', 'ABC') => 'BANC'
```

#### IDEA

```JavaScript
1. Collects the require counts in an array.
2. Iterates through the original string, based on each existing position,
modify another index `left`.
3. Shrink down the substring by increase the left index.
```

#### CODE

```JavaScript
function minWindow(originalString, targetString) {
  let minString = '';
  let minLength = Number.MAX_SAFE_INTEGER;
  let countLength = 0;
  let left = 0;
  /** Builds the require count in an array. */
  const requireCounts = [];
  for (let letter of targetString) {
    requireCounts[letter] = requireCounts[letter] + 1 || 1;
  }
  for (let cur = 0; cur < originalString.length; cur++) {
    const rLetter = originalString[cur];
    /** Decrease the require counts and increase the count. */
    if (requireCounts[rLetter] !== undefined) {
      requireCounts[rLetter] -= 1;
      if (requireCounts[rLetter] >= 0) countLength += 1;
      while (countLength === targetString.length) {
        const currentLength = cur - left + 1;
        if (currentLength < minLength) {
          minLength = currentLength;
          minString = originalString.substr(left, minLength);
        }
        const lLetter = originalString[left];
        if (lLetter !== undefined) {
          requireCounts[lLetter] += 1;
          if (requireCounts[lLetter] > 0) countLength -= 1;
        }
        left++;
      }
    }
  }
  return minString;
}
```
