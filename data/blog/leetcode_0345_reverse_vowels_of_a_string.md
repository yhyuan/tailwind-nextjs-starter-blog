---
title: Leetcode 0345 reverse vowels of a string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0345 reverse vowels of a string in Rust
---

## Solution in Rust

```rust
/**
 * [345] Reverse Vowels of a String
 *
 * Given a string s, reverse only all the vowels in the string and return it.
 * The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both cases.
 *
 * Example 1:
 * Input: s = "hello"
 * Output: "holle"
 * Example 2:
 * Input: s = "leetcode"
 * Output: "leotcede"
 *
 * Constraints:
 *
 * 	1 <= s.length <= 3 * 10^5
 * 	s consist of printable ASCII characters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/reverse-vowels-of-a-string/
// discuss: https://leetcode.com/problems/reverse-vowels-of-a-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn find_start(chars: &Vec<char>, start: i32) -> i32 {
        let mut i = (start + 1) as usize;
        while i < chars.len() && chars[i] != 'a' && chars[i] != 'e' && chars[i] != 'i' && chars[i] != 'o' && chars[i] != 'u'
        && chars[i] != 'A' && chars[i] != 'E' && chars[i] != 'I' && chars[i] != 'O' && chars[i] != 'U' {
            i += 1;
        }
        i as i32
    }
    pub fn find_end(chars: &Vec<char>, end: i32) -> i32 {
        let mut i = (end - 1) as usize;
        //  i >=0 &&
        while chars[i] != 'a' && chars[i] != 'e' && chars[i] != 'i' && chars[i] != 'o' && chars[i] != 'u'
        && chars[i] != 'A' && chars[i] != 'E' && chars[i] != 'I' && chars[i] != 'O' && chars[i] != 'U' {
            //println!("i: {}", i);
            if i == 0 {
                break;
            }
            i -= 1;
        }
        i as i32
    }
    pub fn reverse_vowels(s: String) -> String {
        if s.len() <= 1 {
            return s;
        }
        let mut chars: Vec<char> = s.chars().collect();
        let mut start = Self::find_start(&chars, -1);
        let mut end = Self::find_end(&chars, chars.len() as i32);
        while start < end {
            chars.swap(start as usize, end as usize);
            start = Self::find_start(&chars, start);
            end = Self::find_end(&chars, end);
        }
        chars.iter().collect::<String>()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_345() {
        //assert_eq!(Solution::reverse_vowels("hello".to_string()), "holle".to_string());
        //assert_eq!(Solution::reverse_vowels("leetcode".to_string()), "leotcede".to_string());
        //assert_eq!(Solution::reverse_vowels(" ".to_string()), " ".to_string());
        //assert_eq!(Solution::reverse_vowels("aA".to_string()), "Aa".to_string());
        assert_eq!(Solution::reverse_vowels(".,".to_string()), ".,".to_string());
    }
}

```
