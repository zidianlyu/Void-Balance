# Depth First Search

```JavaScript
Generals:
1. Reduce space complexity.
```

### QUESTION: Generate Parenthesis [lc-22](https://leetcode.com/problems/generate-parentheses/)

---

#### DESCRIPTION

```JavaScript
Generates the parenthesis that for each open, there must be a close.
And it cannot has a close without a open in front.
```

#### IDEA

```JavaScript
1. Init the basic stack to store the tmp string, left and right.
2. when both quotes equal to n, record the path.
3. if right less than left, add right to tmp path.
4. if left is less than n, add left to tmp path.
```

#### CODE

```JavaScript
function generateParenthesisDFS(n) {
  const paths = [];
  const stack = [['', 0, 0]];
  while (stack.length) {
    const [tmp, left, right] = stack.pop();
    if (left === n && right === n) {
      paths.unshift(tmp);
      continue;
    }
    if (right < left) {
      stack.push([tmp + ')', left, right + 1]);
    }
    if (left < n) {
      stack.push([tmp + '(', left + 1, right]);
    }
  }
  return paths;
}
```
