---
title: Leetcode 1035 uncrossed lines
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1035 uncrossed lines in Rust
---

## Solution in Rust

```rust
/**
 * [1035] Uncrossed Lines
 *
 * You are given two integer arrays nums1 and nums2. We write the integers of nums1 and nums2 (in the order they are given) on two separate horizontal lines.
 * We may draw connecting lines: a straight line connecting two numbers nums1[i] and nums2[j] such that:
 *
 * 	nums1[i] == nums2[j], and
 * 	the line we draw does not intersect any other connecting (non-horizontal) line.
 *
 * Note that a connecting line cannot intersect even at the endpoints (i.e., each number can only belong to one connecting line).
 * Return the maximum number of connecting lines we can draw in this way.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/04/26/142.png" style="width: 400px; height: 286px;" />
 * Input: nums1 = [1,4,2], nums2 = [1,2,4]
 * Output: 2
 * Explanation: We can draw 2 uncrossed lines as in the diagram.
 * We cannot draw 3 uncrossed lines, because the line from nums1[1] = 4 to nums2[2] = 4 will intersect the line from nums1[2]=2 to nums2[1]=2.
 *
 * Example 2:
 *
 * Input: nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
 * Output: 3
 *
 * Example 3:
 *
 * Input: nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
 * Output: 2
 *
 *
 * Constraints:
 *
 * 	1 <= nums1.length, nums2.length <= 500
 * 	1 <= nums1[i], nums2[j] <= 2000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/uncrossed-lines/
// discuss: https://leetcode.com/problems/uncrossed-lines/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn helper(nums1: &Vec<i32>, nums2: &Vec<i32>, k1: usize, k2: usize, memo: &mut HashMap<(usize, usize), i32>) -> i32 {
        if memo.contains_key(&(k1, k2)) {
            return memo[&(k1, k2)];
        }
        if nums1.len() == k1 {
            memo.insert((k1, k2), 0);
            return 0i32;
        }
        if nums2.len() == k2 {
            memo.insert((k1, k2), 0);
            return 0i32;
        }
        if nums1[k1] == nums2[k2] {
            let pre_res = Self::helper(nums1, nums2, k1 + 1, k2 + 1, memo);
            memo.insert((k1, k2), pre_res + 1);
            pre_res + 1
        } else {
            let pre_res1 = Self::helper(nums1, nums2, k1, k2 + 1, memo);
            let pre_res2 = Self::helper(nums1, nums2, k1 + 1, k2, memo);
            if pre_res1 > pre_res2 {
                memo.insert((k1, k2 + 1), pre_res1);
                pre_res1
            } else {
                memo.insert((k1 + 1, k2), pre_res2);
                pre_res2
            }
        }
    }
    pub fn max_uncrossed_lines(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let mut memo: HashMap<(usize, usize), i32> = HashMap::new();
        Self::helper(&nums1, &nums2, 0usize, 0usize, &mut memo)
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1035() {
        assert_eq!(Solution::max_uncrossed_lines(vec![1, 4, 2], vec![1, 2, 4]), 2);
        assert_eq!(Solution::max_uncrossed_lines(vec![2,5,1,2,5], vec![10,5,2,1,5,2]), 3);
        assert_eq!(Solution::max_uncrossed_lines(vec![1,3,7,1,7,5], vec![1,9,2,5,1]), 2);
    }
}

```
