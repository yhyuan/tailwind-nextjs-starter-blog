---
title: Leetcode 0011 container with most water
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0011 container with most water in Rust
---

## Solution in Rust

```rust
/**
 * [11] Container With Most Water
 *
 * Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of the line i is at (i, ai) and (i, 0). Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.
 * Notice that you may not slant the container.
 *
 * Example 1:
 * <img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg" style="width: 600px; height: 287px;" />
 * Input: height = [1,8,6,2,5,4,8,3,7]
 * Output: 49
 * Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
 *
 * Example 2:
 *
 * Input: height = [1,1]
 * Output: 1
 *
 * Example 3:
 *
 * Input: height = [4,3,2,1,4]
 * Output: 16
 *
 * Example 4:
 *
 * Input: height = [1,2,1]
 * Output: 2
 *
 *
 * Constraints:
 *
 * 	n == height.length
 * 	2 <= n <= 10^5
 * 	0 <= height[i] <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/container-with-most-water/
// discuss: https://leetcode.com/problems/container-with-most-water/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    #[inline]
    pub fn calculate_area(height: &Vec<i32>, i: usize, j: usize) -> i32 {
        i32::min(height[i], height[j]) * ((j - i) as i32)
    }
    pub fn max_area(height: Vec<i32>) -> i32 {
        if height.len() < 2 {
            return 0;
        }
        let mut i = 0;
        let mut j = height.len() - 1;
        let mut max_area = Solution::calculate_area(&height, i, j);
        while i < j {
            if height[i] > height[j] {
                j = j - 1;
            } else {
                i = i + 1;
            }
            max_area = i32::max(max_area, Solution::calculate_area(&height, i, j));
        }
        max_area
    }
}
*/
impl Solution {
    pub fn max_area(height: Vec<i32>) -> i32 {
        let n = height.len();
        let mut low = 0usize;
        let mut high = n - 1;
        let mut max_area = i32::min(height[0], height[n - 1]) * (n as i32 - 1 - 0);
        while low < high {
            if height[low] > height[high] {
                high = high - 1;
            } else {
                low = low + 1;
            }
            max_area = i32::max(max_area, i32::min(height[low], height[high]) * (high as i32 - low as i32));
        }
        max_area
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_11() {
        assert_eq!(Solution::max_area(vec![1, 8, 6, 2, 5, 4, 8, 3, 7]), 49);
        assert_eq!(Solution::max_area(vec![6, 9]), 6);
        assert_eq!(Solution::max_area(vec![1, 1, 2, 1, 1, 1]), 5);
        assert_eq!(Solution::max_area(vec![5,2,12,1,5,3,4,11,9,4]), 55);
    }
}

```
