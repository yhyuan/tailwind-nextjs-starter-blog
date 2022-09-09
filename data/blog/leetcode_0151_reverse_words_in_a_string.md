---
title: Leetcode 0151 reverse words in a string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0151 reverse words in a string in Rust
---

## Solution in Rust

```rust
/**
 * [151] Reverse Words in a String
 *
 * Given an input string s, reverse the order of the words.
 * A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
 * Return a string of the words in reverse order concatenated by a single space.
 * Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.
 *
 * Example 1:
 *
 * Input: s = "the sky is blue"
 * Output: "blue is sky the"
 *
 * Example 2:
 *
 * Input: s = "  hello world  "
 * Output: "world hello"
 * Explanation: Your reversed string should not contain leading or trailing spaces.
 *
 * Example 3:
 *
 * Input: s = "a good   example"
 * Output: "example good a"
 * Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
 *
 * Example 4:
 *
 * Input: s = "  Bob    Loves  Alice   "
 * Output: "Alice Loves Bob"
 *
 * Example 5:
 *
 * Input: s = "Alice does not even like bob"
 * Output: "bob like even not does Alice"
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 10^4
 * 	s contains English letters (upper-case and lower-case), digits, and spaces ' '.
 * 	There is at least one word in s.
 *
 *
 * <b data-stringify-type="bold">Follow-up: If the string data type is mutable in your language, can you solve it <b data-stringify-type="bold">in-place with <code data-stringify-type="code">O(1) extra space?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/reverse-words-in-a-string/
// discuss: https://leetcode.com/problems/reverse-words-in-a-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn reverse_words(s: String) -> String {
        let mut words: Vec<&str> = s.trim().split_ascii_whitespace().collect();
        words.reverse();
        words.join(" ")
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_151() {
        assert_eq!(
            Solution::reverse_words("the sky is blue".to_owned()),
            "blue is sky the".to_owned()
        );
        assert_eq!(
            Solution::reverse_words("  hello world!  ".to_owned()),
            "world! hello".to_owned()
        );
    }
}

```
