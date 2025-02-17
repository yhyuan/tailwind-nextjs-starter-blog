---
title: Leetcode 0042 trapping rain water
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0042 trapping rain water in Rust
---

## Solution in Rust

```rust
/**
 * [42] Trapping Rain Water
 *
 * Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
 *
 * Example 1:
 * <img src="https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png" style="width: 412px; height: 161px;" />
 * Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
 * Output: 6
 * Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
 *
 * Example 2:
 *
 * Input: height = [4,2,0,3,2,5]
 * Output: 9
 *
 *
 * Constraints:
 *
 * 	n == height.length
 * 	1 <= n <= 2 * 10^4
 * 	0 <= height[i] <= 10^5
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/trapping-rain-water/
// discuss: https://leetcode.com/problems/trapping-rain-water/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

/*
impl Solution {
    pub fn trap(height: Vec<i32>) -> i32 {
        let n = height.len();
        let mut total = 0;
        for i in 1..n-1 {
            let max_left = (0..i).into_iter().map(|k| height[k]).max().unwrap();
            let max_right = (i + 1..n).into_iter().map(|k| height[k]).max().unwrap();
            let min_height = i32::min(max_left, max_right);
            if min_height > height[i] {
                total += min_height - height[i];
            }
        }
        total
    }
}
*/
/*
impl Solution {
    pub fn trap(height: Vec<i32>) -> i32 {
        let n = height.len();
        let mut total = 0;
        let mut max_left: Vec<i32> = vec![0; n];
        let mut max_right: Vec<i32> = vec![0; n];
        for i in 1..n {
            max_left[i] = i32::max(max_left[i - 1], height[i - 1]);
        }
        for i in (0..n-1).rev() {
            max_right[i] = i32::max(max_right[i + 1], height[i + 1]);
        }
        for i in 1..n-1 {
            let max_left_val = max_left[i];
            let max_right_val = max_right[i];
            let min_height = i32::min(max_left_val, max_right_val);
            if min_height > height[i] {
                total += min_height - height[i];
            }
        }
        total
    }
}
*/

impl Solution {
    pub fn trap(height: Vec<i32>) -> i32 {
        let n = height.len();
        if n <= 2 {
            return 0;
        }
        let mut left_peaks = vec![i32::MIN; n];
        left_peaks[0] = height[0];
        let mut peak = height[0];
        for i in 1..n - 1 {
            left_peaks[i] = peak;
            if height[i] >= height[i + 1] && height[i] >= height[i - 1] {
                peak = i32::max(peak, height[i]);
            }
        }
        left_peaks[n - 1] = left_peaks[n - 2];

        let mut right_peaks = vec![i32::MIN; n];
        right_peaks[n - 1] = height[n - 1];
        let mut peak = height[n - 1];
        for i in (1..n - 1).rev() {
            right_peaks[i] = peak;
            if height[i] >= height[i + 1] && height[i] >= height[i - 1] {
                peak = i32::max(peak, height[i]);
            }
        }
        right_peaks[0] = right_peaks[1];

        let mut total = 0;
        for i in 0..n {
            if left_peaks[i] == i32::MIN || right_peaks[i] == i32::MIN {
                continue;
            }
            let v = i32::min(left_peaks[i], right_peaks[i]) - height[i];
            //println!("i: {}, v: {}", i, v);
            if v > 0 {
                total += v;
            }
        }
        //println!("left_peaks: {:?}", left_peaks);
        //println!("right_peaks: {:?}", right_peaks);
        total
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_42() {
        assert_eq!(Solution::trap(vec![9,6,8,8,5,6,3]), 3);

        assert_eq!(Solution::trap(vec![0,1,0,2,1,0,1,3,2,1,2,1]), 6);
        assert_eq!(Solution::trap(vec![4,2,0,3,2,5]), 9);

    }
}

```
