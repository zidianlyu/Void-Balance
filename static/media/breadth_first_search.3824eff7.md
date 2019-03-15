# Breadth First Search

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
1. Init the basic queue to store the tmp string, left remain and right remain.
2. when both remain reduce to 0, record the path.
3. when right remain is more than left remain, add right to tmp path.
4. when left remain is more than 0, add left to tmp path.
```

#### CODE

```JavaScript
function generateParenthesisBFS(n) {
  const paths = [];
  const queue = [['', n, n]];
  while (queue.length) {
    const [tmp, leftRemain, rightRemain] = queue.shift();
    if (!leftRemain && !rightRemain) {
      paths.push(tmp);
      continue;
    }
    if (rightRemain > leftRemain) {
      queue.push([tmp + ')', leftRemain, rightRemain - 1]);
    }
    if (leftRemain > 0) {
      queue.push([tmp + '(', leftRemain - 1, rightRemain]);
    }
  }
  return paths;
}
```
