This implementation searches the search space to valdiate a condition by accessing a single index in the array. The end condition of this implementation is when right + 1 == left. At the end of termination no element is left out for post-processing.

```python
def binarySearch(nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: int
    """
    if len(nums) == 0:
        return -1

    l, r = 0, len(nums) - 1
    while l <= r:
        m = (l + r) // 2
        if nums[m] == target:
            return m
        elif nums[m] < target:
            l = m + 1
        else:
            r = m - 1

    # End Condition: left > right
    return -1
```

# Sqrt(x)

Given a non-negative integer `x`, compute and return *the square root of* `x`. Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

You are not allowed to use any built-in exponent function or operator, such as `pow(x, 0.5)` or `x ** 0.5`.

```markdown
Input: x = 4
Output: 2

Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.


Constraints:
* 0 <= x <= 231 - 1
```

For x > 2, the square root is always in between 0 < x < x / 2. Since we are requested to truncate to integer the problem goes down to iteration over a sorted set of integers.

Sqrt becomes the closest integer to the target if the target is not found eg. 8 -> 2

```python
class Solution:

    def mySqrt(self, x: int) -> int:
        if x < 2:
            return x

        # search space
        l = 0
        r = x // 2
        while l <= r:
            m = (l + r) // 2
            y = m * m
            # check if square of number reaches x
            if y == x:
                return m
            if y > x:
                r = m - 1
            else:
                l = m + 1
        return r
```

# Guess Game

We are playing the Guess Game. The game is as follows: I pick a number from `1` to `n`. You have to guess which number I picked. Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns three possible results:

- `-1`: Your guess is higher than the number I picked (i.e. `num > pick`).
- `1`: Your guess is lower than the number I picked (i.e. `num < pick`).
- `0`: your guess is equal to the number I picked (i.e. `num == pick`).

Return *the number that I picked*.

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        l = 0
        r = n - 1

        while l <= r:
            m = (l + r) // 2
            a = guess(m) 
            if a == 0:
                return m
            if a == -1:
                r = m - 1
            else:
                l = m + 1
        return l
```

# Search in Sorted Rotated Array

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

```markdown
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Input: nums = [1], target = 0
Output: -1
```

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-104 <= target <= 104`

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        n = len(nums)
        if n == 1:
            return 0 if nums[0] == target else -1
        p = self.pivot(nums)
        if nums[p] == target:
            return p
        # array is not pivoted
        if p <= 0:
            # do a simple binary search
            return self.binary(0, n - 1, nums, target)
        if nums[0] > target:
            return self.binary(p, n - 1, nums, target)
        return self.binary(0, p, nums, target)

    def binary(self, l, r, nums, target):
        while l <= r:
            m = (l + r) // 2
            if nums[m] == target:
                return m
            if nums[m] > target:
                r = m - 1
            else:
                l = m + 1
        return -1

    def pivot(self, nums):
        l = 0
        r = len(nums) - 1
        if nums[l] < nums[r]:
            return 0
        while l <= r:
            m = (l + r) // 2
            if nums[m] > nums[m + 1]:
                return m + 1
            else:
                if nums[l] > nums[m]:
                    r = m - 1
                else:
                    l = m + 1
        return -1
```
