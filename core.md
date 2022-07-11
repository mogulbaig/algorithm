Fundamental algorithm to search for a specific target in an ordered collection. 

Binary search operates on a specified left and right index called a Search Space. It maintains left, right, and middle indices of the search space and applies the search condition to the middle value of the collection. If the condition is unsatisfied, that half is eliminated  and the search continues on the remaining half till its successful.

```markdown
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. 
If target exists, then return its index. Otherwise, return -1.You must write an algorithm with O(log n) runtime complexity.


Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4


Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1


Constraints:

* 1 <= nums.length <= 104
* -104 < nums[i], target < 104
* All the integers in nums are unique.
* Nums is sorted in ascending order.
```

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l = 0
        r = len(nums) - 1
        while l <= r:
            m = (l + r) // 2
            if nums[m] == target:
                return m
            if target > nums[m]:
                l = m + 1
            else:
                r = m - 1
        return 0 if nums[0] == target else -1
```
