# Two Pointers

#### Shift + Cmd + v for preview

#### [Two Pointers](https://leetcode.com/tag/two-pointers/)

```Bash
Solves:
1. 时间降唯
```

### 3Sum [lc-15](https://leetcode.com/problems/3sum/)

---

#### Description

```Bash
Given an array nums of n integers,
are there elements a, b, c in nums
such that a + b + c = 0?
Find all unique triplets in the array which gives the sum of zero.
```

#### Example:

```JavaScript
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

#### Idea:

```Bash
1. sort the array
2. transverse the array to get the first num
3. use 2 pointers on the sub-array, and move the pointers based on the cases
```

#### Code:

```JavaScript
function threeSum(nums) {
  const res = [];
  /** Sorts the array in order to use two pointer. */
  nums.sort((a, b) => a - b);

  /** Need to guarantee there are at least 2 positions. */
  for (let i = 0; i < nums.length - 2; i++) {
    /**
     * Cannot change to nums[i] === nums[i + 1] continue,
     * because it might remove a valid `j`
     */
    if (i > 0 && nums[i] === nums[i - 1]) continue;
    let [j, k] = [i + 1, nums.length - 1];
    while (j < k) {
      const sum = nums[i] + nums[j] + nums[k];
      /** Handles too large. */
      if (sum > 0) k--;

      /** Handles too small. */
      if (sum < 0) j++;

      /** Handles result. */
      if (sum === 0) {
        /** Pushes result. */
        res.push([nums[i], nums[j], nums[k]])

        /** Removes duplicates */
        while (j < k && nums[j] === nums[j + 1]) j++;
        while (j < k && nums[k] === nums[k - 1]) k--;

        /** Updates pointers. */
        j++;
        k--;
      }
    }
  }
  return res;
}
```

### 3Sum Closest [lc-16](https://leetcode.com/problems/3sum-closest/)

---

#### Description

```
Given an array nums of n integers and an integer target,
find three integers in nums such that the sum is closest to target.
Return the sum of the three integers.
You may assume that each input would have exactly one solution.
```

#### Example:

```
Given array nums = [-1, 2, 1, -4], and target = 1.
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

#### Idea:

```
1. Sort the array in order to use two pointer
2. set a random min at initial.
3. update the min by cases.
```

#### Code:

```JavaScript
function threeSumClosest(nums, target) {
  nums.sort((a, b) => a - b);
  let min = Number.MAX_SAFE_INTEGER;

  for (let i = 0; i < nums.length - 2; i++) {
    let [j, k] = [i + 1, nums.length - 1];
    while (j < k) {
      const sum = nums[i] + nums[j] + nums[k];

      /** Handles the sum too small. */
      if (sum <= target) j++;

      /** Handles the sum too large. */
      if (sum > target) k--;

      /** Updates the min value */
      if (Math.abs(sum - target) < Math.abs(min - target))
        min = sum;
    }
  }
  return min;
}
```

### 4Sum [lc-18](https://leetcode.com/problems/4sum/)

---

#### Description

```
Given an array nums of n integers and an integer target,
are there elements a, b, c, and d in nums such that
a + b + c + d = target?
Find all unique quadruplets in the array which gives the sum of target.
```

#### Example:

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

#### Idea:

```Bash
Similar to 3Sum
But pay attention!
1. Avoid adding duplicate on i =>
if(i > initial && arr[i] === arr[i - 1]) continue
```

#### Code:

```JavaScript
function fourSum(arr, target) {
  arr.sort((a, b) => a - b);
  const res = [];

  for (let i = 0; i < arr.length - 3; i++) {
    /**
     * Avoid duplicate searching on i.
     * i.e. [0,0,0,0]
     */
    if (i > 0 && arr[i] === arr[i - 1]) continue;
    for (let j = i + 1; j < arr.length - 2; j++) {
      /**
       * Avoid duplicate searching on j.
       * i.e. [-3, -2, -1, 0, 0, 1, 2, 3]
      */
      if (j > i + 1 && arr[j] === arr[j - 1]) continue;
      let [k, l] = [j + 1, arr.length - 1];
      while (k < l) {
        const sum = arr[i] + arr[j] + arr[k] + arr[l];
        if (sum < target) k++;
        if (sum > target) l--;
        if (sum === target) {
          res.push([arr[i], arr[j], arr[k], arr[l]]);
          while (k < l && arr[k] === arr[k + 1]) k++;
          while (k < l && arr[l] === arr[l - 1]) l--;
          k++;
          l--;
        }
      }
    }
  }
  return res;
}
```
