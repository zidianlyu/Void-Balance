# Mathematics

```JavaScript
Generals:
1. Optimize time and space.
2. Usage of String and Array.
```

### QUESTION: Build a simple calculator [lc-227](https://leetcode.com/problems/basic-calculator-ii/)

---

#### DESCRIPTION

```JavaScript
Build a function that can process the string to finish the calculation
For example, '3 + 3 * 10' => 33
```

#### IDEA

```JavaScript
1. Process the string from start to end.
2. Use a stack to store the partial result from all calculation.
3. Sum numbers from the stack.
```

#### SETPS

```JavaScript
Use a stack to remember the partials calculated number
Use a sign to remember the last operator, default to '+'
Use a number to accumulate the current calculation result.
If a calculation happened, update the operand.
```

#### CASES

```JavaScript
letter is number
letter is ' '
letter is sign
the last letter
```

#### LOGIC

```JavaScript
If the letter is a number AND not a space, increase the tmp number.
If the letter is a sign OR we are facing the last letter, do the operation.
```

#### CAREFUL

```JavaScript
Number(' ') is count for 0 in JavaScript.
Math.trunc(number) can cut the trailing digits of a number.
```

#### CODE

```JavaScript
function calculator(s) {
  const stack = [];
  let num = 0;
  let sign = '+';
  for (let i = 0; i < s.length; i++) {
    const letter = s[i];
    if (!isNaN(letter) && letter !== ' ') num = num * 10 + Number(letter);
    if (isNaN(letter) || i === s.length - 1) {
      switch (sign) {
        case '+':
          stack.push(num);
          break;
        case '-':
          stack.push(-num);
          break;
        case '*':
          stack.push(stack.pop() * num);
          break;
        case '/':
          stack.push(Math.trunc(stack.pop() / num));
          break;
      }
      num = 0;
      sign = letter;
    }
  }
  if (!stack.length) return -1;
  return stack.reduce((a, b) => a + b);
}
```

### Question Basic Calculator[lc-224](https://leetcode.com/problems/basic-calculator/)

---

#### Description

```JavaScript
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ),
the plus + or minus sign -, non-negative integers and empty spaces .
```

#### Example:

```JavaScript
Input: "1 + 1"
Output: 2

Input: " 2-1 + 2 "
Output: 3

Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

#### Idea:

```JavaScript
Use a stack to handle the parenthesis case.
Use a result to carry the addition
Use a sign to check '+' or '-'
```

```JavaScript
var calculate = function(s) {
  const stack = [];
  let [result, sign] = [0, 1];
  for (let i = 0; i < s.length; i++) {
    if (isDigit(s[i])) {
      let num = s[i] | 0;
      while (i < s.length - 1 && isDigit(s[i + 1])) {
        num = num * 10 + (s[i + 1] | 0);
        i++;
      }
      result += num * sign;
    } else if (s[i] === '+')
      sign = 1;
    else if (s[i] === '-')
      sign = -1;
    else if (s[i] === '(') {
      stack.push(result);
      stack.push(sign);
      [result, sign] = [0, 1];
    } else if (s[i] === ')') {
      const previousSign = stack.pop();
      const previousResult = stack.pop();
      result = previousSign * result + previousResult;
    }
  }
  return result;

};

function isDigit(c) {
  return c !== ' ' && !isNaN(c);
}
```
