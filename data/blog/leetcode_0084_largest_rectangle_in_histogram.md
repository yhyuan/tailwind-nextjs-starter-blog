---
title: Leetcode 0084 largest rectangle in histogram
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0084 largest rectangle in histogram in Rust
---

## Solution in Rust

```rust
/**
 * [84] Largest Rectangle in Histogram
 *
 * Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg" style="width: 522px; height: 242px;" />
 * Input: heights = [2,1,5,6,2,3]
 * Output: 10
 * Explanation: The above is a histogram where width of each bar is 1.
 * The largest rectangle is shown in the red area, which has an area = 10 units.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg" style="width: 202px; height: 362px;" />
 * Input: heights = [2,4]
 * Output: 4
 *
 *
 * Constraints:
 *
 * 	1 <= heights.length <= 10^5
 * 	0 <= heights[i] <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/largest-rectangle-in-histogram/
// discuss: https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    /*
    pub fn largest_rectangle_area(heights: Vec<i32>) -> i32 {
        let n = heights.len();
        //let mut min_matrix: Vec<Vec<i32>> = vec![vec![0; n]; n];
        let mut largest_area = i32::MIN;
        for i in 0..n {
            let mut row: Vec<i32> = vec![0; n];
            for j in i..n {
                if i == j {
                    //min_matrix[i][i] = heights[i];
                    row[j] = heights[i];
                } else {
                    //min_matrix[i][j] = i32::min(min_matrix[i][j - 1], heights[j]);
                    row[j] = i32::min(row[j - 1], heights[j]);
                }
                //let area = min_matrix[i][j] * ((j - i + 1) as i32);
                let area = row[j] * ((j - i + 1) as i32);
                largest_area = i32::max(largest_area, area);
            }
        }
        largest_area
    }
    */
    /*
    pub fn largest_rectangle_area(heights: Vec<i32>) -> i32 {
        let mut stack: Vec<i32> = vec![];
        let mut i = 0;
        let mut max_area = 0;
        while i < heights.len() {
            if stack.len() == 0 || heights[i] > stack[stack.len() - 1] {
                stack.push(heights[i]);
            } else {
                let curr = stack.pop().unwrap();
                let width = if stack.len() == 0 {1usize} else {i - (stack[stack.len() - 1] as usize) + 1};
                max_area = i32::max(max_area, (width as i32) * heights[curr as usize]);
                i -= 1;
            }
            i += 1;
        }
        max_area
        //0i32
    }
    */
    /*
    pub fn largest_rectangle_area(heights: Vec<i32>) -> i32 {
        let n = heights.len();
        let mut largest_area = i32::MIN;
        for i in 0..n {
            for j in i..n {
                let min_height = (i..=j).into_iter().map(|k| heights[k]).min().unwrap();
                let area = min_height * ((j - i + 1) as i32);
                largest_area = i32::max(largest_area, area);
            }
        }
        largest_area
    }
    */
    pub fn largest_rectangle_area(heights: Vec<i32>) -> i32 {
        let n = heights.len();
        let mut leftest_less_indices: Vec<i32> = vec![-1; n];
        leftest_less_indices[0] = -1;
        for i in 1..n {
            if heights[i] <= heights[i - 1] {
                let mut index = leftest_less_indices[i - 1];
                while index >= 0 && heights[index as usize] >= heights[i] {
                    index = leftest_less_indices[index as usize];
                }
                leftest_less_indices[i] = index as i32;
            } else {
                leftest_less_indices[i] = i as i32 - 1;
            }
        }
        let mut rightest_less_indices: Vec<i32> = vec![-1; n];
        rightest_less_indices[n - 1] = n as i32;
        for i in (0..n-1).rev() {
            if heights[i] <= heights[i + 1] {
                let mut index = rightest_less_indices[i + 1];
                while index < n as i32 && heights[index as usize] >= heights[i] {
                    index = rightest_less_indices[index as usize];
                }
                rightest_less_indices[i] = index as i32;
            } else {
                rightest_less_indices[i] = i as i32 + 1;
            }
        }
        let mut largest_area = i32::MIN;
        for i in 0..n {
            let area = heights[i] * (rightest_less_indices[i] - leftest_less_indices[i] - 1);
            largest_area = i32::max(largest_area, area);
        }
        largest_area
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_84() {
        assert_eq!(Solution::largest_rectangle_area(vec![2, 1, 5, 6, 2, 3]), 10);
        assert_eq!(
            Solution::largest_rectangle_area(vec![1, 1, 1, 1, 1, 1, 1, 1]),
            8
        );
        assert_eq!(Solution::largest_rectangle_area(vec![2, 2]), 4);
        assert_eq!(
            Solution::largest_rectangle_area(vec![1, 2, 8, 8, 2, 2, 1]),
            16
        );
        assert_eq!(Solution::largest_rectangle_area(vec![2, 1, 2]), 3);
        assert_eq!(Solution::largest_rectangle_area(vec![1, 3, 2, 1, 2]), 5);
    }
}

```
