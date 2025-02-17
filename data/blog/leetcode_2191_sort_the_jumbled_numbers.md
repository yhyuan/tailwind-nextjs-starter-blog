---
title: Leetcode 2191 sort the jumbled numbers
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 2191 sort the jumbled numbers in Rust
---

## Solution in Rust

```rust
/*
2191. Sort the Jumbled Numbers
Medium

88

9

Add to List

Share
You are given a 0-indexed integer array mapping which represents the mapping rule of a shuffled decimal system. mapping[i] = j means digit i should be mapped to digit j in this system.

The mapped value of an integer is the new integer obtained by replacing each occurrence of digit i in the integer with mapping[i] for all 0 <= i <= 9.

You are also given another integer array nums. Return the array nums sorted in non-decreasing order based on the mapped values of its elements.

Notes:

Elements with the same mapped values should appear in the same relative order as in the input.
The elements of nums should only be sorted based on their mapped values and not be replaced by them.


Example 1:

Input: mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]
Output: [338,38,991]
Explanation:
Map the number 991 as follows:
1. mapping[9] = 6, so all occurrences of the digit 9 will become 6.
2. mapping[1] = 9, so all occurrences of the digit 1 will become 9.
Therefore, the mapped value of 991 is 669.
338 maps to 007, or 7 after removing the leading zeros.
38 maps to 07, which is also 7 after removing leading zeros.
Since 338 and 38 share the same mapped value, they should remain in the same relative order, so 338 comes before 38.
Thus, the sorted array is [338,38,991].
Example 2:

Input: mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]
Output: [123,456,789]
Explanation: 789 maps to 789, 456 maps to 456, and 123 maps to 123. Thus, the sorted array is [123,456,789].


Constraints:

mapping.length == 10
0 <= mapping[i] <= 9
All the values of mapping[i] are unique.
1 <= nums.length <= 3 * 104
0 <= nums[i] < 109

*/
// use std::collections::HashMap;
pub struct Solution {}

impl Solution {
    pub fn sort_jumbled(mapping: Vec<i32>, nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut nums = nums;
        let max_num = nums.iter().max().unwrap();
        let size = format!("{}", max_num).len();
        let mut indices = (0..n).into_iter().map(|i| i).collect::<Vec<_>>();
        let mut mapped_nums = vec![0i32; n];
        for i in 0..n {
            let num = nums[i];
            let size = format!("{}", num).len();
            let mut power_n = 1;
            for j in 0..size {
                mapped_nums[i] +=  mapping[((num % (power_n * 10)) / power_n) as usize] * power_n;
                power_n = power_n * 10;
            }
        }
        // println!("{:?}", mapped_nums);
        let mut power_n = 1;
        for i in 0..size {
            let digits = mapped_nums.iter()
                .map(|&num| ((num % (power_n * 10)) / power_n))
                .collect::<Vec<_>>();
            // println!("digits: {:?}", digits);
            let mut freq: Vec<Vec<usize>> = vec![];
            for k in 0..10 {
                freq.push(vec![]);
            }
            // let mut freq: [Vec<usize>; 10] = [Vector::new(); 10];
            for k in 0..n {
                let index = indices[k];
                let digit = digits[index];
                freq[digit as usize].push(index);
            }
            // println!("indices: {:?}", indices);
            let mut index = 0;
            for k in 0..10 {
                for m in 0..freq[k].len() {
                    indices[index] = freq[k][m];
                    index += 1;
                }
            }
            power_n = power_n * 10;
        }
        //println!("indices: {:?}", indices);
        let mut result = vec![0; n];
        for i in 0..n {
            result[i] = nums[indices[i]];
        }
        result
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_2191() {
        assert_eq!(Solution::sort_jumbled(vec![8,9,4,0,2,1,3,5,7,6], vec![991,338,38]), vec![338,38,991]);
    }
}


```
