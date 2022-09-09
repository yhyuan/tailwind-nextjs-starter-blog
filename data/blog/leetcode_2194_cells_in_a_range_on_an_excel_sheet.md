---
title: Leetcode 2194 cells in a range on an excel sheet
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 2194 cells in a range on an excel sheet in Rust
---

## Solution in Rust

```rust
/*
2194. Cells in a Range on an Excel Sheet
Easy

51

19

Add to List

Share
A cell (r, c) of an excel sheet is represented as a string "<col><row>" where:

<col> denotes the column number c of the cell. It is represented by alphabetical letters.
For example, the 1st column is denoted by 'A', the 2nd by 'B', the 3rd by 'C', and so on.
<row> is the row number r of the cell. The rth row is represented by the integer r.
You are given a string s in the format "<col1><row1>:<col2><row2>", where <col1> represents the column c1, <row1> represents the row r1, <col2> represents the column c2, and <row2> represents the row r2, such that r1 <= r2 and c1 <= c2.

Return the list of cells (x, y) such that r1 <= x <= r2 and c1 <= y <= c2. The cells should be represented as strings in the format mentioned above and be sorted in non-decreasing order first by columns and then by rows.



Example 1:


Input: s = "K1:L2"
Output: ["K1","K2","L1","L2"]
Explanation:
The above diagram shows the cells which should be present in the list.
The red arrows denote the order in which the cells should be presented.
Example 2:


Input: s = "A1:F1"
Output: ["A1","B1","C1","D1","E1","F1"]
Explanation:
The above diagram shows the cells which should be present in the list.
The red arrow denotes the order in which the cells should be presented.


Constraints:

s.length == 5
'A' <= s[0] <= s[3] <= 'Z'
'1' <= s[1] <= s[4] <= '9'
s consists of uppercase English letters, digits and ':'.
*/
// use std::collections::HashMap;
pub struct Solution {}

impl Solution {
    pub fn cells_in_range(s: String) -> Vec<String> {
        let chars: Vec<char> = s.chars().collect();
        let col1 = chars[0];
        let row1 = chars[1];
        let col2 = chars[3];
        let row2 = chars[4];
        let mut result: Vec<String> = vec![];
        let mut i = col1;
        while i <= col2 {
            let mut j = row1;
            while j <= row2 {
                result.push(format!("{}{}", i, j));
                j = (j as u8 + 1) as char;
            }
            i = (i as u8 + 1) as char;
        }
        result
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_2194() {
        assert_eq!(Solution::cells_in_range("K1:L2".to_string()), vec_string!["K1","K2","L1","L2"]);
    }
}


```
