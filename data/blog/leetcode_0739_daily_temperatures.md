---
title: Leetcode 0739 daily temperatures
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0739 daily temperatures in Rust
---

## Solution in Rust

```rust
/**
 * [739] Daily Temperatures
 *
 * Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the i^th day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead.
 *
 * Example 1:
 * Input: temperatures = [73,74,75,71,69,72,76,73]
 * Output: [1,1,4,2,1,1,0,0]
 * Example 2:
 * Input: temperatures = [30,40,50,60]
 * Output: [1,1,1,0]
 * Example 3:
 * Input: temperatures = [30,60,90]
 * Output: [1,1,0]
 *
 * Constraints:
 *
 * 	1 <= temperatures.length <= 10^5
 * 	30 <= temperatures[i] <= 100
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/daily-temperatures/
// discuss: https://leetcode.com/problems/daily-temperatures/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn daily_temperatures(temperatures: Vec<i32>) -> Vec<i32> {
        let n = temperatures.len();
        let mut result: Vec<i32> = vec![0; n];
        let mut stack: Vec<(usize, i32)> = vec![];
        for i in 0..n - 1 {
            if temperatures[i + 1] > temperatures[i] {
                result[i] = 1;
                if stack.len() > 0 {
                    let (mut index, mut value) = stack[stack.len() - 1];
                    while stack.len() > 0 && temperatures[i] > value {
                        result[index] = (i - index) as i32;
                        stack.pop();
                        if stack.len() > 0 {
                            let top = stack[stack.len() - 1];
                            index = top.0;
                            value = top.1;
                        }
                    }
                    while stack.len() > 0 && temperatures[i + 1] > value {
                        result[index] = (i + 1 - index) as i32;
                        stack.pop();
                        if stack.len() > 0 {
                            let top = stack[stack.len() - 1];
                            index = top.0;
                            value = top.1;
                        }
                    }
                }
            } else {
                stack.push((i, temperatures[i]));
            }
        }
        result
    }
}

/*
impl Solution {
    pub fn daily_temperatures(temperatures: Vec<i32>) -> Vec<i32> {
        let n = temperatures.len();
        let mut result: Vec<i32> = vec![0; n];
        for i in 0..n {
            for j in i + 1..n {
                if temperatures[j] > temperatures[i] {
                    result[i] = (j - i) as i32;
                    break;
                }
            }
        }
        result
    }
}

*/
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_739() {
        assert_eq!(Solution::daily_temperatures(vec![73,74,75,71,69,72,76,73]), vec![1,1,4,2,1,1,0,0]);
        assert_eq!(Solution::daily_temperatures(vec![30,40,50,60]), vec![1,1,1,0]);
        assert_eq!(Solution::daily_temperatures(vec![30,60,90]), vec![1,1,0]);
    }
}

```
