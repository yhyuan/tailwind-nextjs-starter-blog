---
title: Leetcode 0273 integer to english words
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0273 integer to english words in Rust
---

## Solution in Rust

```rust
/**
 * [273] Integer to English Words
 *
 * Convert a non-negative integer num to its English words representation.
 *
 * Example 1:
 * Input: num = 123
 * Output: "One Hundred Twenty Three"
 * Example 2:
 * Input: num = 12345
 * Output: "Twelve Thousand Three Hundred Forty Five"
 * Example 3:
 * Input: num = 1234567
 * Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
 * Example 4:
 * Input: num = 1234567891
 * Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"
 *
 * Constraints:
 *
 * 	0 <= num <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/integer-to-english-words/
// discuss: https://leetcode.com/problems/integer-to-english-words/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn number_to_words(num: i32) -> String {
        let numbers: [&str; 20] = ["Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine",
                                    "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"];
        let ten_numbers: [&str; 10] = ["", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"];
        if num == 0 {
            return "Zero".to_string();
        }
        if num < 1000 {
            let hundred = num / 100;
            let result = if hundred == 0 {"".to_string()} else {format!("{} Hundred", numbers[hundred as usize])};
            let remain = num % 100;
            if remain == 0 {
                return result;
            }
            if remain <= 19 {
                if result.len() == 0 {
                    return format!("{}", numbers[remain as usize]);
                } else {
                    return format!("{} {}", result, numbers[remain as usize]);
                }
            }
            let ten = (remain / 10) as usize;
            let one = (remain % 10) as usize;
            let mut items: Vec<String> = vec![ten_numbers[ten].to_string()];
            if one > 0 {
                items.push(numbers[one].to_string());
            }
            if result.len() > 0 {
                items.insert(0, result);
            }
            let result: String = items.join(" ");
            return result;
        }
        if num < 1000000 {
            let thousand = Solution::number_to_words(num / 1000);
            if num % 1000 == 0 {
                return format!("{} Thousand", thousand);
            }
            let remain = Solution::number_to_words(num % 1000);
            return format!("{} Thousand {}", thousand, remain);
        }
        if num < 1000000000 {
            let million = Solution::number_to_words(num / 1000000);
            if num % 1000000 == 0 {
                return format!("{} Million", million);
            }
            let remain = Solution::number_to_words(num % 1000000);
            return format!("{} Million {}", million, remain);
        }
        let billion = Solution::number_to_words(num / 1000000000);
        if num % 1000000000 == 0 {
            return format!("{} Billion", billion);
        }
        let remain = Solution::number_to_words(num % 1000000000);
        return format!("{} Billion {}", billion, remain);
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_273() {
        //2,147,483,648
        //1,234,567,891
        //1,234,567
        //12,345

        assert_eq!(Solution::number_to_words(1000010), "One Million Ten".to_string());
        assert_eq!(Solution::number_to_words(1000), "One Thousand".to_string());
        assert_eq!(Solution::number_to_words(20), "Twenty".to_string());
        assert_eq!(Solution::number_to_words(0), "Zero".to_string());
        assert_eq!(Solution::number_to_words(123), "One Hundred Twenty Three".to_string());
        assert_eq!(Solution::number_to_words(12345), "Twelve Thousand Three Hundred Forty Five".to_string());
        assert_eq!(Solution::number_to_words(1234567), "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven".to_string());
        assert_eq!(Solution::number_to_words(1234567891), "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One".to_string());
    }
}

```
