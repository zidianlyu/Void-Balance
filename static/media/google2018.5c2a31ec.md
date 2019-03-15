### 739. Daily Temperatures
```JavaScript
/**
 * Method 1:
 * 1. traverse through the array, for each position i, find i + 1 -> end
 *
 * Time: O(n^2), Space: O(n)
 */
function daysToWarmer(arr) {
  const res = [];
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] > arr[i]) {
        res.push(j - i);
        break;
      }
      if (j === arr.length - 1) {
        res.push(0);
      }
    }

  }
  res.push(0);
  return res;
}

console.log(daysToWarmer([73, 74, 75, 71, 69, 72, 76, 73]));

/**
 * Method 2
 * 1. traverse the arr from back, keep record the large number in a stack.
 * 2. if cur is larger or equals to the last of the stack, pop from stack.
 * 3. for each step, if stack is not empty, then get the position diff.
 * stack is used to record the day with the highest temperature
 * [76, 72, 69...]
 * Time O(n), space(2n)
 */
function daysToWarmer(arr) {
  const res = [];
  const stack = [];
  for (let i = arr.length - 1; i >= 0; i--) {
    /** If cur > last number from stack, pop it. */
    while (stack.length && arr[i] >= arr[stack[stack.length - 1]]) {
      stack.pop();
    }

    /** The last number of the stack should be larger than current.  */
    res[i] = stack.length ? stack[stack.length - 1] - i : 0;

    /** En-stack the index from tail. */
    stack.push(i)
  }

  return res;
}
console.log(daysToWarmer([73, 74, 75, 71, 69, 72, 76, 73]));
```

```JavaScript
/**
 *
 * if input num is less than 1, then is false;
 * if input num can be divide by 3, i.e. 6, 15, 45 then we can keep divide
 * all the way to the end. And the end case 3 / 3 = 1
 * Finally check whether the end case is equals to 1 or not.
 */
function powerOfThree(n) {
  if (n < 1) return false;
  while (n % 3 === 0) {
    n /= 3;
  }
  return n === 1;
}
console.log(powerOfThree(3) === true);
console.log(powerOfThree(9) === true);
console.log(powerOfThree(27) === true);
console.log(powerOfThree(4) === false);
console.log(powerOfThree(0) === false);
```


```JavaScript
/**
 * Use two pointers to identify whether a string is palindrome or not.
 * use i <= j to cover the case such as 'aba'
 */
function isPalindrome(s) {
  let [i, j] = [0, s.length - 1];
  while (i <= j) {
    if (s[i] !== s[j]) return false;
    i++;
    j--;
  }
  return true;
}
console.log(isPalindrome('racecar'));
console.log(isPalindrome('aaabbaaa'));
```

```JavaScript
/**
 * # 406
 * Mark refers to how many previous people is taller than the current.
 * 1. use heights to record the variety of height
 * 2. use a dict to record [height: {mark, pos(originalPosition)}]
 * 3. traverse heights from tall to short
 * 4. sort the dict[height], and insert into the res[].
 * `mark` in this case means how many previous people before this node is taller
 * `pos` is the original position.
 * Therefor, get the element from original position and insert into res[]
 * by the mark
 */
/**
 *
 * Careful:
 * Insert in JavaScript
 * splice(i, 0, element)
 */
function reArrangeQueue(inputQueue) {
  const heightMarkMap = {};
  const heights = [];
  for (let pos = 0; pos < inputQueue.length; pos++) {
    const [height, mark] = inputQueue[pos];
    if (height in heightMarkMap) {
      heightMarkMap[height].push([mark, pos]);
    } else {
      heightMarkMap[height] = [[mark, pos]];
      heights.push(height);
    }
  }
  res = [];
  for (const height of heights.sort((a, b) => b - a)) {
    const marks = heightMarkMap[height].sort((a, b) => a - b);
    for (const [mark, pos] of marks) {
      res.splice(mark, 0, inputQueue[pos]);
    }
  }
  return res;
}
console.log(reArrangeQueue([[7, 0], [4, 4], [7, 1], [5, 0], [6, 1], [5, 2]]));
```

```JavaScript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}
const root = new TreeNode(5);
root.left = new TreeNode(4);
root.right = new TreeNode(8);
root.left.left = new TreeNode(11);
root.right.left = new TreeNode(13);
root.right.right = new TreeNode(4);
root.left.left.left = new TreeNode(7);
root.left.left.right = new TreeNode(2);
root.right.right.right = new TreeNode(1);
/**
 * # 22.
 * Back Tracking
 * 1. tracking the remaining sum.
 * 2. through to the child to check.
 * Time: O(n), space: O(n)
 */
function hasPathSum(root, sum) {
  if (!root) return false;
  sum -= root.val;
  if (!root.left && !root.right) {
    return sum === 0;
  }
  return hasPathSum(root.left, sum) || hasPathSum(root.right, sum);
}
//  console.log(hasPathSum(root, 97));
/**
 * # 22.
 * BFS
 * 1. match the remaining sum value to the node value.
 * 2. build the queue by traverse in level.
 * Time: O(n), space: O(n)
 */
function hasPathSumBFS(root, sum) {
  const queue = [[root, sum]];
  while (queue.length) {
    const [node, sum] = queue.shift();
    if (!node.left && !node.right && sum === node.val) {
      return true;
    }
    if (node.left) {
      queue.push([node.left, sum - node.val]);
    }
    if (node.right) {
      queue.push([node.right, sum - node.val]);
    }
  }
  return false;
}
// console.log(hasPathSumBFS(root, 29));
/**
 * # 22.
 * DFS
 * 1. match the remaining sum value to the node value.
 * 2. build the stack in depth
 * 3. traverse the left side first.
 * Time: O(n), space: O(n)
 */
function hasPathSumDFS(root, sum) {
  const stack = [[root, sum]];
  while (stack.length) {
    const [node, sum] = stack.pop();
    if (!node.left && !node.right && sum === node.val) {
      return true;
    }
    if (node.right) {
      stack.push([node.right, sum - node.val]);
    }
    if (node.left) {
      stack.push([node.left, sum - node.val]);
    }
  }
  return false;
}
console.log(hasPathSumDFS(root, 22));
```

```JavaScript
/**
 * #463
 * 1. add the count size by 4 whenever find a block.
 * 2. reduce it by 1 if find any neighbors.
 * Time: O(n), space: O(1)
 */
function calculateIslandPerimeter(islands) {
  let count = 0;
  for (let i = 0; i < islands.length; i++) {
    for (let j = 0; j < islands[i].length; j++) {
      if (islands[i][j]) {
        count += 4;
        // UP
        if (i > 0 && islands[i - 1][j]) count -= 1;
        // DOWN
        if (i < islands.length - 1 && islands[i + 1][j]) count -= 1;
        // LEFT
        if (j > 0 && islands[i][j - 1]) count -= 1;
        // RIGHT
        if (j < islands[i].length - 1 && islands[i][j + 1]) count -= 1;
      }
    }
  }
  return count;
}
const board = [[0, 1, 0, 0], [1, 1, 1, 0], [0, 1, 0, 0], [1, 1, 0, 0]];
console.log(calculateIslandPerimeter(board));
```

```JavaScript
/**
 * # 448
 * Given that the number will be sit in range 1 -> n
 * 1. In a traverse, set all the duplicate numbers to negative. (find duplicate)
 * 2. In another traverse, find all the positive (find non-duplicate pos)
 * 3. Mark the non duplicate location to the actually numbers (which is i + 1)
 */
/**
 * Thoughts:
 * 1. When not allow to use extra space, it implies to use the index.
 * 2. Times a number by -1 in some sense would not affect it, but also marks it.
 */
function findMissingNumber(arr) {
  // Records the duplicate numbers by index.
  for (let i = 0; i < arr.length; i++) {
    // Mark the duplicate numbers.
    const index = Math.abs(arr[i]) - 1;
    arr[index] = Math.abs(arr[index]) * -1;
  }
  for (let i = 0; i < arr.length; i++) {
    // Converts the index back to the actual number.
    if (arr[i] > 0) arr[i] = i + 1;
  }
  return arr.filter(num => num > 0);
}
console.log(findMissingNumber([1, 2, 6, 7, 8, 3, 3, 2]));
```

```JavaScript
/**
 * Build an array of length n, return the last number
 * DP, build the current number base on the previous square number
 * Base case, the first number, 0
 * Inductive: An = A1 + A4 + A9 + ...
 */
function perfectSquare(n) {
  const dp = [0]
  while (dp.length <= n) {
    /** Gets the length of the current array */
    const m = dp.length;

    /** Build the minimum number. */
    let min = Number.MAX_SAFE_INTEGER;

    /** Traverses all the square number in the front. */
    for (let i = 1; i * i <= m; i++) {
      const squareNumber = dp[m - i * i];
      min = Math.min(min, squareNumber + 1);
    }
    dp.push(min);
  }
  return dp.pop();
}

console.log(perfectSquare(12));
```


```JavaScript
/**
 * use a map to store {word, index}
 * match all
 * case 1: ''
 * case 2: 'abc', 'cba'
 *
 * partial
 * case 3: 'aabc', 'cbca'
 * case 4: 'abca', 'caba'
 */
// 11:20
function palindromePairs(arr) {
  const map = new Map();
  for (let i = 0; i < arr.length; i++) {
    map.set(arr[i], i);
  }

  const res = [];
  // case 1, ''
  if (map.has('')) {
    const emptyIndex = map.get('');
    for (let i = 0; i < arr.length; i++) {
      if (isPalindrome(arr[i]) && arr[i] !== '') {
        res.push([emptyIndex, i]);
        res.push([i, emptyIndex]);
      }
    }
  }

  // case 2, 'abc', 'bca'
  for (let i = 0; i < arr.length; i++) {
    const word = arr[i];
    const reverseWord = reverse(word);
    if (map.has(reverseWord) && map.get(reverseWord) !== i) {
      res.push([i, map.get(reverseWord)]);
    }
  }

  // case 3, 4
  for (let i = 0; i < arr.length; i++) {
    const word = arr[i];
    for (let j = 1; j < word.length; j++) {
      // abca, cba
      if (isPalindrome(word.substring(j))) {
        const head = word.substring(0, j);
        const reverseHead = reverse(head);
        if (map.has(reverseHead) && map.get(reverseHead) !== i) {
          res.push([i, map.get(reverseHead)]);
        }
      }

      // aabc, cba
      if (isPalindrome(word.substring(0, j))) {
        const tail = word.substring(j);
        const reverseTail = reverse(tail);
        if (map.has(reverseTail) && map.get(reverseTail) !== i) {
          res.push([map.get(reverseTail), i]);
        }
      }
    }
  }
  return res;
}

function reverse(s) {
  return s.split('').reverse().join('');
}

function isPalindrome(s) {
  let [i, j] = [0, s.length - 1];
  while (i <= j) {
    if (s[i] !== s[j]) return false;
    i++;
    j--;
  }
  return true;
}

console.log(palindromePairs(["abcd", "dcba", "lls", "s", "sssll"]));
```


```JavaScript
/**
 * use String.charCodeAt to convert char into number
 * use a array to store the frequency of number display
 * check the array, if the frequency equals to 1, return the index.
 * Time O(n), space O(n)
 */
function firstUniqueChar(s) {
  const arr = [];
  for (const letter of s) arr[num(letter)] = arr[num(letter)] + 1 || 1;
  for (let i = 0; i < s.length; i++) {
    if (arr[num(s[i])] === 1) return i;
  }
  return -1
}

function num(c) {
  return c.charCodeAt();
}

console.log(firstUniqueChar('cca'));
```



```JavaScript
/**
 * use two pointers to check the vowel
 * case 1, if head is not vowel, i++
 * case 2, if head is vowel,
 *  if tail is not vowel, j--
 *  if tail is vowel, i++, j--
 * Time: O(n), space: O(n)
 */
function reverseVowels(s) {
  const arr = s.split('');
  let [i, j] = [0, s.length - 1];
  while (i <= j) {
    if (!isVowel(arr[i])) i++;
    else {
      if (i !== j && !isVowel(arr[j])) j--;
      else {
        [arr[i], arr[j]] = [arr[j], arr[i]];
        i++;
        j--
      }
    }
  }
  return arr.join('');
}

function isVowel(s) {
  return ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'].includes(s);
}

console.log(reverseVowels('hello'));
```


```JavaScript
/**
 * Robot Path
 * use a fix array to remember the co-related path 'U' and 'R mean +1,
 * otherwise, -1
 * traverse the record, if there is non-zero, return false;
 */
var judgeCircle = function(moves) {
  const count = [0, 0];
  for (const move of moves) {
    switch (move) {
      case 'U':
        count[0] += 1;
        break;
      case 'D':
        count[0] -= 1;
        break;
      case 'R':
        count[1] += 1;
        break;
      case 'L':
        count[1] -= 1;
        break;
    }
  }

  for (let num of count) {
    if (num !== 0) return false;
  }
  return true;
};
```



```JavaScript
/**
 * Use two pointers to check the existence.
 * start from the top right of the board.
 * if the target is smaller than current, move it left, j--;
 * if the target is greater than current, move it down, i++;
 *
 * It will essential go to the edge no matter what.
 * Time: O(m + n), space: O(1)
 */
var searchMatrix = function(board, target) {
  if (!board.length) return false;
  let [i, j] = [0, board[0].length - 1];
  while (j >= 0 && i < board.length) {
    if (target < board[i][j]) {
      j--;
    } else if (target > board[i][j]) {
      i++;
    } else {
      return true;
    }
  }
  return false;
};
```

```JavaScript
/**
 * use an arr to count the frequency of the letters
 * use a stack to record the result
 * use a set to track the visited
 *
 * case 1, if the letter has not been visited
 *
 * lexical concern,
 * if the cur is less then the last of the stack(cur should be in the front),
 * and the last of the stack has more duplicate,
 * remove the last from stack,
 * as well as visited(since they can be add in the future)
 *
 * after add to stack, should add to visited
 *
 * Time: O(n), space: O(n)
 */
function removeDuplicateLetters(s) {
  const arr = new Array(26).fill(0);
  for (const c of s) arr[num(c)] += 1;

  const stack = [];
  const visited = new Set();
  for (const c of s) {
    arr[num(c)] -= 1;
    if (visited.has(c)) continue;
    while (stack.length) {
      const l = last(stack);
      if (num(l) > num(c) && arr[num(l)] > 0) {
        stack.pop();
        visited.delete(l);
      } else {
        break;
      }
    }
    stack.push(c);
    visited.add(c);
  }
  return stack.join('');
}

function num(c) {
  return c.charCodeAt() - 'a'.charCodeAt();
}

function last(stack) {
  return stack[stack.length - 1];
}

console.log(removeDuplicateLetters('cbacdcbc'));
```
