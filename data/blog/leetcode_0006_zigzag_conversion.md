---
title: Leetcode 0006 zigzag conversion
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0006 zigzag conversion in Rust
---

## Solution in Rust

```rust
/**
 * [6] ZigZag Conversion
 *
 * The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
 *
 * P   A   H   N
 * A P L S I I G
 * Y   I   R
 *
 * And then read line by line: "PAHNAPLSIIGYIR"
 * Write the code that will take a string and make this conversion given a number of rows:
 *
 * string convert(string s, int numRows);
 *
 *
 * Example 1:
 *
 * Input: s = "PAYPALISHIRING", numRows = 3
 * Output: "PAHNAPLSIIGYIR"
 *
 * Example 2:
 *
 * Input: s = "PAYPALISHIRING", numRows = 4
 * Output: "PINALSIGYAHRPI"
 * Explanation:
 * P     I    N
 * A   L S  I G
 * Y A   H R
 * P     I
 *
 * Example 3:
 *
 * Input: s = "A", numRows = 1
 * Output: "A"
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 1000
 * 	s consists of English letters (lower-case and upper-case), ',' and '.'.
 * 	1 <= numRows <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/zigzag-conversion/
// discuss: https://leetcode.com/problems/zigzag-conversion/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn convert(s: String, num_rows: i32) -> String {
        if num_rows == 1 {
            return s;
        }
        let chars: Vec<char> = s.chars().collect();
        let n = num_rows as usize;
        // Calculate row length
        let row_size = (chars.len() / (n + n - 2) + 1) * (n - 1);
        let mut table: Vec<Vec<char>> = vec![vec![' '; row_size]; n];
        let mut k = 0;
        let mut row = 0;
        let mut col = 0;
        let mut is_increasing = true;
        while k < chars.len() {
            table[row][col] = chars[k];
            if row < n - 1 && row > 0 {
                if is_increasing {
                    row += 1;
                } else {
                    row -= 1;
                    col += 1;
                }
            } else if row == n - 1 {
                row -= 1;
                col += 1;
                is_increasing = false;
            } else {
                row += 1;
                is_increasing = true;
            }
            k += 1;
        }
        let mut chars: Vec<char> = vec![];
        for i in 0..table.len() {
            for j in 0..table[0].len() {
                if table[i][j] != ' ' {
                    chars.push(table[i][j]);
                }
            }
        }
        //println!("{:?}", table);
        let result: String = chars.iter().collect();
        result
        //String::new()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_6() {
        /*
        assert_eq!(
            Solution::convert("PAYPALISHIRING".to_string(), 4),
            "PINALSIGYAHRPI"
        );
        assert_eq!(
            Solution::convert("PAYPALISHIRING".to_string(), 3),
            "PAHNAPLSIIGYIR"
        );
        assert_eq!(Solution::convert("A".to_string(), 1), "A");
        */
        assert_eq!(Solution::convert("AY".to_string(), 2), "AY");
    }
}

```
