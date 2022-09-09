---
title: Leetcode 0014 longest common prefix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0014 longest common prefix in Rust
---

## Solution in Rust

```rust
/**
 * [14] Longest Common Prefix
 *
 * Write a function to find the longest common prefix string amongst an array of strings.
 * If there is no common prefix, return an empty string "".
 *
 * Example 1:
 *
 * Input: strs = ["flower","flow","flight"]
 * Output: "fl"
 *
 * Example 2:
 *
 * Input: strs = ["dog","racecar","car"]
 * Output: ""
 * Explanation: There is no common prefix among the input strings.
 *
 *
 * Constraints:
 *
 * 	1 <= strs.length <= 200
 * 	0 <= strs[i].length <= 200
 * 	strs[i] consists of only lower-case English letters.
 *
 */
pub struct Solution {}
//use std::collections::HashSet;
// problem: https://leetcode.com/problems/longest-common-prefix/
// discuss: https://leetcode.com/problems/longest-common-prefix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn longest_common_prefix(strs: Vec<String>) -> String {
        if strs.len() == 0 {
            return "".to_string();
        }
        let chars_vector: Vec<Vec<char>> = strs.iter().map(|str| str.chars().collect()).collect();
        let smallest_size = chars_vector.iter().map(|chars| chars.len()).min().unwrap();
        //println!("{:?}", chars_vector);
        //println!("{:?}", sizes);
        let mut result: Vec<char> = vec![];

        for i in 0..smallest_size {
            let chars: Vec<char> = chars_vector.iter().map(|chars| chars[i]).collect();
            let mut is_same = true;
            if chars.len() > 1 {
                for i in 1..chars.len() {
                    if chars[i - 1] != chars[i] {
                        is_same = false;
                        break;
                    }
                }
            }
            if is_same {
                result.push(chars[0]);
            } else {
                break;
            }
        }

        result.iter().collect()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_14() {
        assert_eq!(
            Solution::longest_common_prefix(vec![
                "".to_string(),
                "racecar".to_string(),
                "car".to_string()
            ]),
            ""
        );

        assert_eq!(
            Solution::longest_common_prefix(vec![
                "flower".to_string(),
                "flow".to_string(),
                "flight".to_string()
            ]),
            "fl"
        );
        assert_eq!(Solution::longest_common_prefix(vec![]), "");

    }
}

```
