---
title: Leetcode 0402 remove k digits
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0402 remove k digits in Rust
---

## Solution in Rust

```rust
/**
 * [402] Remove K Digits
 *
 * Given string num representing a non-negative integer num, and an integer k, return the smallest possible integer after removing k digits from num.
 *
 * Example 1:
 *
 * Input: num = "1432219", k = 3
 * Output: "1219"
 * Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
 *
 * Example 2:
 *
 * Input: num = "10200", k = 1
 * Output: "200"
 * Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
 *
 * Example 3:
 *
 * Input: num = "10", k = 2
 * Output: "0"
 * Explanation: Remove all the digits from the number and it is left with nothing which is 0.
 *
 *
 * Constraints:
 *
 * 	1 <= k <= num.length <= 10^5
 * 	num consists of only digits.
 * 	num does not have any leading zeros except for the zero itself.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/remove-k-digits/
// discuss: https://leetcode.com/problems/remove-k-digits/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn remove_kdigits(num: String, k: i32) -> String {
        if k == 0 {
            return num;
        }
        if num.len() == 1 {
            return "0".to_string();
        }
        let chars: Vec<char> = num.chars().collect();
        let n = chars.len();
        let mut i = 0;
        while i < n - 1 && chars[i] <= chars[i + 1] {
            i += 1;
        }
        //println!("i: {}", i);
        if i == 0 {
            let mut j = 1;
            while chars[j] == '0' {
                j += 1;
                if j >= n {
                    break;
                }
            }
            if j == n {
                return "0".to_string();
            }
            let new_num = format!("{}", &num[j..n]);
            return Self::remove_kdigits(new_num, k - 1);
        }
        if i == n - 1 {
            let new_num = format!("{}", &num[0..n - 1]);
            return Self::remove_kdigits(new_num, k - 1);
        }
        let new_num = format!("{}{}", &num[0..i], &num[i + 1..n]);
        return Self::remove_kdigits(new_num, k - 1);
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_402() {
        //assert_eq!(Solution::remove_kdigits("1432219".to_string(), 3), "1219".to_string());
        //assert_eq!(Solution::remove_kdigits("10200".to_string(), 1), "200".to_string());
        //assert_eq!(Solution::remove_kdigits("10".to_string(), 2), "0".to_string());
        assert_eq!(Solution::remove_kdigits("112".to_string(), 1), "11".to_string());
    }
}

```
