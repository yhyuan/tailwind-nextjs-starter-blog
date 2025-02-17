---
title: Leetcode 0386 lexicographical numbers
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0386 lexicographical numbers in Rust
---

## Solution in Rust

```rust
/**
 * [386] Lexicographical Numbers
 *
 * Given an integer n, return all the numbers in the range [1, n] sorted in lexicographical order.
 * You must write an algorithm that runs in O(n) time and uses O(1) extra space.
 *
 * Example 1:
 * Input: n = 13
 * Output: [1,10,11,12,13,2,3,4,5,6,7,8,9]
 * Example 2:
 * Input: n = 2
 * Output: [1,2]
 *
 * Constraints:
 *
 * 	1 <= n <= 5 * 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/lexicographical-numbers/
// discuss: https://leetcode.com/problems/lexicographical-numbers/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn lexical_order(n: i32) -> Vec<i32> {
        if n <= 9 {
            return (1..=n).into_iter().collect::<Vec<i32>>();
        }
        let m = format!("{}", n).len();
        let mut nums: Vec<i32> = vec![];
        let mut k = 0;
        let n = n as u32;
        let pow_val = 10u32.pow(m as u32 - 1);
        for i in pow_val..=n {
            for j in (1..m as u32).rev() {
                let pow = 10u32.pow(j);
                if i % pow == 0 {
                    nums.push(i as i32 / pow as i32);
                    k += 1;
                }
            }
            nums.push(i as i32);
        }
        let pre_nums = Self::lexical_order(pow_val as i32 - 1);
        for i in k..pre_nums.len() {
            nums.push(pre_nums[i]);
        }
        nums
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_386() {
        assert_eq!(Solution::lexical_order(13), vec![1,10,11,12,13,2,3,4,5,6,7,8,9]);
        assert_eq!(Solution::lexical_order(2), vec![1,2]);
        assert_eq!(Solution::lexical_order(100), vec![1,10,100,11,12,13,14,15,16,17,18,19,2,20,21,22,23,24,25,26,27,28,29,3,30,31,32,33,34,35,36,37,38,39,4,40,41,42,43,44,45,46,47,48,49,5,50,51,52,53,54,55,56,57,58,59,6,60,61,62,63,64,65,66,67,68,69,7,70,71,72,73,74,75,76,77,78,79,8,80,81,82,83,84,85,86,87,88,89,9,90,91,92,93,94,95,96,97,98,99]);
    }
}

//"9", "90", "900", "901", "902", "903", "904", "905", "906", "907", "908", "909", "91", "910", "911", "912", "913", "914", "915", "916", "917", "918", "919", "92", "920", "921", "922", "923", "924", "925", "926", "927", "928", "929", "93", "930", "931", "932", "933", "934", "935", "936", "937", "938", "939", "94", "940", "941", "942", "943", "944", "945", "946", "947", "948", "949", "95", "950", "951", "952", "953", "954", "955", "956", "957", "958", "959", "96", "960", "961", "962", "963", "964", "965", "966", "967", "968", "969", "97", "970", "971", "972", "973", "974", "975", "976", "977", "978", "979", "98", "980", "981", "982", "983", "984", "985", "986", "987", "988", "989", "99", "990", "991", "992", "993", "994", "995", "996", "997", "998", "999"
//"1", "10", "11", "12", "13", "14", "15", "16", "17", "18", "19"
//"9", "90", "91", "92", "93", "94", "95", "96", "97", "98", "99"
//"8", "80", "800", "801", "802", "803", "804", "805", "806", "807", "808", "809", "81", "810", "811", "812", "813", "814", "815", "816", "817", "818", "819", "82", "820", "821", "822", "823", "824", "825", "826", "827", "828", "829", "83", "830", "831", "832", "833", "834", "835", "836", "837", "838", "839", "84", "840", "841", "842", "843", "844", "845", "846", "847", "848", "849", "85", "850", "851", "852", "853", "854", "855", "856", "857", "858", "859", "86", "860", "861", "862", "863", "864", "865", "866", "867", "868", "869", "87", "870", "871", "872", "873", "874", "875", "876", "877", "878", "879", "88", "880", "881", "882", "883", "884", "885", "886", "887", "888", "889", "89", "890", "891", "892", "893", "894", "895", "896", "897", "898", "899"
```
