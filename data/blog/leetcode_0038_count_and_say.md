---
title: Leetcode 0038 count and say
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0038 count and say in Rust
---

## Solution in Rust

```rust
/**
 * [38] Count and Say
 *
 * The count-and-say sequence is a sequence of digit strings defined by the recursive formula:
 *
 * 	countAndSay(1) = "1"
 * 	countAndSay(n) is the way you would "say" the digit string from countAndSay(n-1), which is then converted into a different digit string.
 *
 * To determine how you "say" a digit string, split it into the minimal number of groups so that each group is a contiguous section all of the same character. Then for each group, say the number of characters, then say the character. To convert the saying into a digit string, replace the counts with a number and concatenate every saying.
 * For example, the saying and conversion for digit string "3322251":
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/23/countandsay.jpg" style="width: 581px; height: 172px;" />
 * Given a positive integer n, return the n^th term of the count-and-say sequence.
 *
 * Example 1:
 *
 * Input: n = 1
 * Output: "1"
 * Explanation: This is the base case.
 *
 * Example 2:
 *
 * Input: n = 4
 * Output: "1211"
 * Explanation:
 * countAndSay(1) = "1"
 * countAndSay(2) = say "1" = one 1 = "11"
 * countAndSay(3) = say "11" = two 1's = "21"
 * countAndSay(4) = say "21" = one 2 + one 1 = "12" + "11" = "1211"
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 30
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/count-and-say/
// discuss: https://leetcode.com/problems/count-and-say/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn count_and_say(n: i32) -> String {
        if n == 1 {
            return "1".to_string();
        }
        let string = Solution::count_and_say(n - 1);
        let digitals = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
        let mut pre_ch = ' ';
        let mut count = 0usize;
        let mut chars: Vec<char> = vec![];
        for (i, ch) in string.chars().enumerate() {
            if i == 0 {
                pre_ch = ch;
                count += 1;
            } else {
                if pre_ch == ch {
                    count += 1;
                } else {
                    chars.push(digitals[count]);
                    chars.push(pre_ch);
                    count = 1;
                    pre_ch = ch;
                }
            }
        }
        chars.push(digitals[count]);
        chars.push(pre_ch);
        chars.into_iter().collect()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_38() {
        assert_eq!(Solution::count_and_say(1), "1");
        assert_eq!(Solution::count_and_say(2), "11");
        assert_eq!(Solution::count_and_say(3), "21");
        assert_eq!(Solution::count_and_say(4), "1211");
        assert_eq!(Solution::count_and_say(5), "111221");
    }
}

```
