---
title: Leetcode 0395 longest substring with at least k repeating characters
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0395 longest substring with at least k repeating characters in Rust
---

## Solution in Rust

```rust
/**
 * [395] Longest Substring with At Least K Repeating Characters
 *
 * Given a string s and an integer k, return the length of the longest substring of s such that the frequency of each character in this substring is greater than or equal to k.
 *
 * Example 1:
 *
 * Input: s = "aaabb", k = 3
 * Output: 3
 * Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.
 *
 * Example 2:
 *
 * Input: s = "ababbc", k = 2
 * Output: 5
 * Explanation: The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 10^4
 * 	s consists of only lowercase English letters.
 * 	1 <= k <= 10^5
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/
// discuss: https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn longest_substring(s: String, k: i32) -> i32 {
        let n = s.len();
        let chars: Vec<char> = s.chars().collect();
        let mut result = 0i32;
        for i in 0..n {
            let mut hashmap: HashMap<char, i32> = HashMap::new();
            for j in i..n {
                let ch = chars[j];
                if hashmap.contains_key(&ch) {
                    hashmap.insert(ch, hashmap[&ch] + 1);
                    let min_freq = hashmap.iter().map(|(_, &v)| v).min().unwrap();
                    if min_freq >= k {
                        result = i32::max(result, (j - i + 1) as i32);
                    }
                } else {
                    hashmap.insert(ch, 1);
                    let min_freq = 1;
                    if min_freq >= k {
                        result = i32::max(result, (j - i + 1) as i32);
                    }
                }
            }
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_395() {
        assert_eq!(Solution::longest_substring("aaabb".to_string(), 3), 3);
        assert_eq!(Solution::longest_substring("ababbc".to_string(), 2), 5);
        assert_eq!(Solution::longest_substring("aaabbb".to_string(), 3), 6);
        assert_eq!(Solution::longest_substring("weitong".to_string(), 2), 0);
        assert_eq!(Solution::longest_substring("bbaaacbd".to_string(), 3), 3);
    }
}

```
