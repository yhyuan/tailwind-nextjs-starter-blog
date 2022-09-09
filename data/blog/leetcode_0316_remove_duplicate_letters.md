---
title: Leetcode 0316 remove duplicate letters
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0316 remove duplicate letters in Rust
---

## Solution in Rust

```rust
/**
 * [316] Remove Duplicate Letters
 *
 * Given a string s, remove duplicate letters so that every letter appears once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.
 *
 * Example 1:
 *
 * Input: s = "bcabc"
 * Output: "abc"
 *
 * Example 2:
 *
 * Input: s = "cbacdcbc"
 * Output: "acdb"
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 10^4
 * 	s consists of lowercase English letters.
 *
 *
 * Note: This question is the same as 1081: <a href="https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/" target="_blank">https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/</a>
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/remove-duplicate-letters/
// discuss: https://leetcode.com/problems/remove-duplicate-letters/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn remove_duplicate_letters(s: String) -> String {
        String::new()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_316() {
    }
}

```
