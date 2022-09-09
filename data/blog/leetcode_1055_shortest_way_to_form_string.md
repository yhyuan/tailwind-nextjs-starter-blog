---
title: Leetcode 1055 shortest way to form string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1055 shortest way to form string in Rust
---

## Solution in Rust

```rust
/**
1055. Shortest Way to Form String
Medium

811

48

Add to List

Share
A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

Given two strings source and target, return the minimum number of subsequences of source such that their concatenation equals target. If the task is impossible, return -1.



Example 1:

Input: source = "abc", target = "abcbc"
Output: 2
Explanation: The target "abcbc" can be formed by "abc" and "bc", which are subsequences of source "abc".
Example 2:

Input: source = "abc", target = "acdbc"
Output: -1
Explanation: The target string cannot be constructed from the subsequences of source string due to the character "d" in target string.
Example 3:

Input: source = "xyz", target = "xzyxz"
Output: 3
Explanation: The target string can be constructed as follows "xz" + "y" + "xz".


Constraints:

1 <= source.length, target.length <= 1000
source and target consist of lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/shortest-way-to-form-string/
// discuss: https://leetcode.com/problems/shortest-way-to-form-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
//sanet.st/books/tag/computers-internet-programming/
use std::collections::HashMap;
impl Solution {
    pub fn shortest_way(source: String, target: String) -> i32 {
        let s_chars = source.chars().collect::<Vec<_>>();
        let n = source.len();
        let mut hashmaps: Vec<HashMap<char, usize>> = vec![HashMap::new(); n];
        for i in (0..n).rev() {
            let ch = s_chars[i];
            let mut next_hashmap = if i < n - 1 {
                hashmaps[i + 1].clone()
            } else {
                HashMap::new()
            };
            next_hashmap.insert(ch, i);
            hashmaps[i] = next_hashmap;
        }
        let mut idx = 0;
        let mut res = 1; // point to source
        for ch in target.chars() {
            if !hashmaps[0].contains_key(&ch) {
                return -1;
            }
            if idx == source.len() || !hashmaps[idx].contains_key(&ch) {
                res += 1;
                idx = 0; // move back
            }
            idx = hashmaps[idx][&ch] + 1;
        }
        res
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1055() {
        assert_eq!(Solution::shortest_way("abc".to_string(), "abcbc".to_string()), 2);
        assert_eq!(Solution::shortest_way("abc".to_string(), "acdbc".to_string()), -1);
        assert_eq!(Solution::shortest_way("xyz".to_string(), "xzyxz".to_string()), 3);
    }
}

```
