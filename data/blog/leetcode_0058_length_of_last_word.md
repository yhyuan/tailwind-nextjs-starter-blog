---
title: Leetcode 0058 length of last word
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0058 length of last word in Rust
---

## Solution in Rust

```rust
/**
 * [58] Length of Last Word
 *
 * Given a string s consisting of some words separated by some number of spaces, return the length of the last word in the string.
 * A word is a maximal substring consisting of non-space characters only.
 *
 * Example 1:
 *
 * Input: s = "Hello World"
 * Output: 5
 * Explanation: The last word is "World" with length 5.
 *
 * Example 2:
 *
 * Input: s = "   fly me   to   the moon  "
 * Output: 4
 * Explanation: The last word is "moon" with length 4.
 *
 * Example 3:
 *
 * Input: s = "luffy is still joyboy"
 * Output: 6
 * Explanation: The last word is "joyboy" with length 6.
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 10^4
 * 	s consists of only English letters and spaces ' '.
 * 	There will be at least one word in s.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/length-of-last-word/
// discuss: https://leetcode.com/problems/length-of-last-word/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn length_of_last_word(s: String) -> i32 {
        let mut chars: Vec<char> = s.trim().chars().collect();
        chars.reverse();
        for i in 0..chars.len() {
            if chars[i] == ' ' {
                return i as i32;
            }
        }
        chars.len() as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_58() {
        assert_eq!(Solution::length_of_last_word("Hello World".to_owned()), 5);
        assert_eq!(Solution::length_of_last_word("       ".to_owned()), 0);
        assert_eq!(Solution::length_of_last_word("".to_owned()), 0);
        assert_eq!(Solution::length_of_last_word("     rrrrr  ".to_owned()), 5);
    }
}

```
