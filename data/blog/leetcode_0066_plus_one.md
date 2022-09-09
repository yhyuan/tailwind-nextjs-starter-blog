---
title: Leetcode 0066 plus one
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0066 plus one in Rust
---

## Solution in Rust

```rust
/**
 * [66] Plus One
 *
 * Given a non-empty array of decimal digits representing a non-negative integer, increment one to the integer.
 * The digits are stored such that the most significant digit is at the head of the list, and each element in the array contains a single digit.
 * You may assume the integer does not contain any leading zero, except the number 0 itself.
 *
 * Example 1:
 *
 * Input: digits = [1,2,3]
 * Output: [1,2,4]
 * Explanation: The array represents the integer 123.
 *
 * Example 2:
 *
 * Input: digits = [4,3,2,1]
 * Output: [4,3,2,2]
 * Explanation: The array represents the integer 4321.
 *
 * Example 3:
 *
 * Input: digits = [0]
 * Output: [1]
 *
 *
 * Constraints:
 *
 * 	1 <= digits.length <= 100
 * 	0 <= digits[i] <= 9
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/plus-one/
// discuss: https://leetcode.com/problems/plus-one/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn plus_one(digits: Vec<i32>) -> Vec<i32> {
        let length = digits.len();
        let mut digits = digits;
        let mut over_flow = false;
        digits[length - 1] = digits[length - 1] + 1;
        for i in (0..length).rev() {
            if digits[i] >= 10 {
                digits[i] = digits[i] - 10;
                if i == 0 {
                    over_flow = true;
                } else {
                    digits[i - 1] = digits[i - 1] + 1;
                }
            }
        }
        if over_flow {
            digits.insert(0, 1);
        }
        digits
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_66() {
        assert_eq!(vec![1,2,4], Solution::plus_one(vec![1,2,3]));
        assert_eq!(vec![4,3,2,2], Solution::plus_one(vec![4,3,2,1]));
        assert_eq!(vec![1], Solution::plus_one(vec![0]));
    }
}

```
