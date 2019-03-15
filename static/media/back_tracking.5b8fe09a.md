# Back Tracking

```JavaScript
Generals:
1. Large question with no clear solution.
2. For tree and map.
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
1. if the right remain is more than the left remain, keep add right.
2. if the left remain is more than 0, keep add left.
3. record the path, if both remaining reduce to 0.
```

#### CODE

```JavaScript
function generateParenthesis(n) {
  const paths = []
  getPaths(paths, '', n, n);
  return paths;
}
function getPaths(paths, tmp, leftRemain, rightRemain) {
  if (!leftRemain && !rightRemain) {
    paths.push(tmp);
    return;
  }
  if (rightRemain > leftRemain) {
    getPaths(paths, tmp + ')', leftRemain, rightRemain - 1);
  }
  if (leftRemain > 0) {
    getPaths(paths, tmp + '(', leftRemain - 1, rightRemain);
  }
  return;
}
```

### QUESTION: N-Queen [lc-51](https://leetcode.com/problems/n-queens/)

---

#### DESCRIPTION

```JavaScript
Given a n x n board, get the possible ways that can set
n Queens on the board, so that they cannot harm each other.
Records the path.
```

#### IDEA

```JavaScript
1. Iterates each column and try placing a Queen.
2. Apply recursion to place Queen if valid, if a path is cut,
then revert to the last step
2. Validation based on the rule. (up-left, up, up-right)
```

#### STEPS

```JavaScript
End case: mark queen on the last row of the board.
Recursion: mark on the grid if it satisfy the validation.
BackTrack: mark the grid back to '' if the path is dead.
```

#### CAREFUL

```JavaScript
Return in the end case.
Validation pointer.
```

#### CODE

```JavaScript
function getQueenPaths(n) {
  const board = new Array(n).fill().map(_ => new Array(n).fill(''));
  const paths = [];
  getPaths(board, paths, 0);
  return paths;
}
function getPaths(board, paths, row) {
  if (row === board.length) {
    return paths.push(JSON.stringify(board.slice()));
  }
  for (let col = 0; col < board[0].length; col++) {
    if (validate(board, row, col)) {
      board[row][col] = '*';
      getPaths(board, paths, row + 1);
      board[row][col] = '';
    }
  }
  return paths;
}
function validate(board, row, col) {
  /** Up-left. */
  for (
    let i = row - 1, j = col - 1;
    i >= 0 && j >= 0;
    i-- , j--) {
    if (board[i][j]) return false;
  }
  /** Up. */
  for (
    let i = row - 1;
    i >= 0;
    i--) {
    if (board[i][col]) return false;
  }
  /** Up-right. */
  for (
    let i = row - 1, j = col + 1;
    i >= 0 && j < board[0].length;
    i-- , j++) {
    if (board[i][j]) return false;
  }
  return true;
}
```

### QUESTION: Sudoku Solver [lc-37](https://leetcode.com/problems/sudoku-solver/)

---

#### DESCRIPTION

```JavaScript
Solve the sudoku.
```

#### IDEA

```JavaScript
1. Iterate through the board.
2. Chekc if the num has not been modified.
3. Use 1 - 9 to try on each step.
4. Validate the number on the given step, if success, keep recursion.
5. Otherwise, restore the state.
```

#### CAREFUL

```JavaScript
Use operator to cut the trailing digits
2.3333 | 0 === 2
arr.some(el => el === condition)
```

#### CODE

```JavaScript
const BOARD = new Array(9).fill().map(_ => new Array(9).fill(0));
function solveSudoku(board) {
  for (let i = 0; i < board.length; i++) {
    for (let j = 0; j < board[i].length; j++) {
      if (board[i][j]) continue;
      for (let n = 1; n < 10; n++) {
        if (validate(board, i, j, n)) {
          board[i][j] = n;
          if (solveSudoku(board)) {
            return board;
          }
        }
      }
      board[i][j] = 0;
      return false;
    }
  }
  return true;
}

function validate(board, row, col, num) {
  if (board[row].some(n => n === num)) return false;
  if (board.map(row => row[col]).some(n => n === num)) return false;
  [row, col] = [(row / 3 | 0) * 3, (col / 3 | 0) * 3];
  for (let i = row; i < row + 3; i++) {
    for (let j = col; j < col + 3; j++) {
      if (board[i][j] === num) return false;
    }
  }
  return true;
}

```
