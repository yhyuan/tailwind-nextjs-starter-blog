---
title: Leetcode 0189 rotate array
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0189 rotate array in Rust
---

## Solution in Rust

```rust
/**
 * [189] Rotate Array
 *
 * Given an array, rotate the array to the right by k steps, where k is non-negative.
 *
 * Example 1:
 *
 * Input: nums = [1,2,3,4,5,6,7], k = 3
 * Output: [5,6,7,1,2,3,4]
 * Explanation:
 * rotate 1 steps to the right: [7,1,2,3,4,5,6]
 * rotate 2 steps to the right: [6,7,1,2,3,4,5]
 * rotate 3 steps to the right: [5,6,7,1,2,3,4]
 *
 * Example 2:
 *
 * Input: nums = [-1,-100,3,99], k = 2
 * Output: [3,99,-1,-100]
 * Explanation:
 * rotate 1 steps to the right: [99,-1,-100,3]
 * rotate 2 steps to the right: [3,99,-1,-100]
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^5
 * 	-2^31 <= nums[i] <= 2^31 - 1
 * 	0 <= k <= 10^5
 *
 *
 * Follow up:
 *
 * 	Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
 * 	Could you do it in-place with O(1) extra space?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/rotate-array/
// discuss: https://leetcode.com/problems/rotate-array/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

/*
impl Solution {
    pub fn rotate(nums: &mut Vec<i32>, k: i32) {
        let n = nums.len();
        let k = k as usize;
        let mut backup: Vec<i32> = vec![0; n];
        for i in 0..n {
            backup[i] = nums[i];
        }
        for i in 0..n {
            nums[(k + i) % n] = backup[i];
        }
    }
}
*/
/*
impl Solution {
    pub fn reverse(nums: &mut Vec<i32>, start: usize, end: usize) {
        if end == start {
            return;
        }
        let mut i = 0usize;
        while end - i > start + i {
            let temp = nums[start + i];
            nums[start + i] = nums[end - i];
            nums[end - i] = temp;
            i += 1;
        }
    }
    pub fn rotate(nums: &mut Vec<i32>, k: i32) {
        let n = nums.len();
        let k = (k as usize) % n;
        if k == 0 {
            return;
        }
        if k == 1 {
            let temp = nums[n - 1];
            for i in (0..n - 1).rev() {
                nums[i + 1] = nums[i];
            }
            nums[0] = temp;
            return;
        }
        //println!("k: {}", k);
        Solution::reverse(nums, 0, n - 1);
        Solution::reverse(nums, 0, k - 1);
        Solution::reverse(nums, k, n - 1);
    }
}
*/
impl Solution {
    pub fn reverse(nums: &mut Vec<i32>, i: usize, j: usize) {
        let mut i = i;
        let mut j = j;
        while i < j {
            nums.swap(i, j);
            i += 1;
            j -= 1;
        }
    }
    pub fn rotate(nums: &mut Vec<i32>, k: i32) {
        let k = k % nums.len() as i32;
        if k == 0 {
            return;
        }
        if nums.len() == 1 {
            return;
        }
        Self::reverse(nums, 0, nums.len() - 1);
        Self::reverse(nums, 0, k as usize - 1);
        Self::reverse(nums, k as usize, nums.len() - 1);
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_189() {
        /*
        let mut nums = vec![1, 2, 3, 4, 5, 6, 7];
        Solution::rotate(&mut nums, 3);
        assert_eq!(nums,        vec![5, 6, 7, 1, 2, 3, 4]);
        let mut nums = vec![1, 2, 3, 4, 5, 6];
        Solution::rotate(&mut nums, 2);
        assert_eq!(nums, vec![5, 6, 1, 2, 3, 4]);
        */
        let mut nums = vec![1];
        Solution::rotate(&mut nums, 1);
        assert_eq!(nums, vec![1]);
    }
}

```
