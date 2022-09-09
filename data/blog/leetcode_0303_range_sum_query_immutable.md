---
title: Leetcode 0303 range sum query immutable
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0303 range sum query immutable in Rust
---

## Solution in Rust

```rust
/**
 * [303] Range Sum Query - Immutable
 *
 * Given an integer array nums, handle multiple queries of the following type:
 * <ol>
 * 	Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
 * </ol>
 * Implement the NumArray class:
 *
 * 	NumArray(int[] nums) Initializes the object with the integer array nums.
 * 	int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).
 *
 *
 * Example 1:
 *
 * Input
 * ["NumArray", "sumRange", "sumRange", "sumRange"]
 * [[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
 * Output
 * [null, 1, -1, -3]
 * Explanation
 * NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
 * numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
 * numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
 * numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^4
 * 	-10^5 <= nums[i] <= 10^5
 * 	0 <= left <= right < nums.length
 * 	At most 10^4 calls will be made to sumRange.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/range-sum-query-immutable/
// discuss: https://leetcode.com/problems/range-sum-query-immutable/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

struct NumArray {
    nums: Vec<i32>
}


/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl NumArray {
    fn new(nums: Vec<i32>) -> Self {
        let mut new_nums: Vec<i32> = Vec::with_capacity(nums.len());
        let mut total = 0;
        for &num in nums.iter() {
            total += num;
            new_nums.push(total);
        }
        NumArray{nums: new_nums}
        //NumArray{nums: nums}
    }

    fn sum_range(&self, left: i32, right: i32) -> i32 {
        let left = left as usize;
        let right = right as usize;
        let diff = if left >= 1 {self.nums[left - 1]} else {0};
        self.nums[right] - diff
        /*
        let left = left as usize;
        let right = right as usize;
        (left..=right).into_iter().map(|i| self.nums[i]).sum()
        */
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * let obj = NumArray::new(nums);
 * let ret_1: i32 = obj.sum_range(left, right);
 */

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_303() {
        let mut nums = NumArray::new(vec![-2, 0, 3, -5, 2, -1]);
        assert_eq!(nums.sum_range(0, 2), 1);
        assert_eq!(nums.sum_range(2, 5), -1);
        assert_eq!(nums.sum_range(0, 5), -3);
    }
}

```
