---
title: Leetcode 0646 maximum length of pair chain
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0646 maximum length of pair chain in Rust
---

## Solution in Rust

```rust
/**
 * [646] Maximum Length of Pair Chain
 *
 * You are given an array of n pairs pairs where pairs[i] = [lefti, righti] and lefti < righti.
 * A pair p2 = [c, d] follows a pair p1 = [a, b] if b < c. A chain of pairs can be formed in this fashion.
 * Return the length longest chain which can be formed.
 * You do not need to use up all the given intervals. You can select pairs in any order.
 *
 * Example 1:
 *
 * Input: pairs = [[1,2],[2,3],[3,4]]
 * Output: 2
 * Explanation: The longest chain is [1,2] -> [3,4].
 *
 * Example 2:
 *
 * Input: pairs = [[1,2],[7,8],[4,5]]
 * Output: 3
 * Explanation: The longest chain is [1,2] -> [4,5] -> [7,8].
 *
 *
 * Constraints:
 *
 * 	n == pairs.length
 * 	1 <= n <= 1000
 * 	-1000 <= lefti < righti < 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/maximum-length-of-pair-chain/
// discuss: https://leetcode.com/problems/maximum-length-of-pair-chain/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn helper(pairs: &Vec<Vec<i32>>, memo: &mut HashMap<usize, i32>, index: usize) -> i32 {
        if index >= pairs.len() {
            return 0;
        }
        if memo.contains_key(&index) {
            return memo[&index];
        }
        let n = pairs.len();
        if index == n - 1 {
            memo.insert(index, 1);
            return 1i32;
        }
        let mut res = Self::helper(pairs, memo, index + 1); // index is ignored
        let pair_end = pairs[index][1];
        let next_index = pairs.iter().position(|pair| pair[0] > pair_end);
        res = i32::max(res, if next_index.is_none() {1} else {Self::helper(pairs, memo, next_index.unwrap()) + 1});
        memo.insert(index, res);
        res
    }
    pub fn find_longest_chain(pairs: Vec<Vec<i32>>) -> i32 {
        let mut pairs = pairs;
        pairs.sort_by(|a, b| a[0].partial_cmp(&b[0]).unwrap());
        let mut memo: HashMap<usize, i32> = HashMap::new();

        Self::helper(&pairs, &mut memo, 0)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_646() {
        assert_eq!(Solution::find_longest_chain(vec![vec![1,2],vec![2,3],vec![3,4]]), 2);
        assert_eq!(Solution::find_longest_chain(vec![vec![1,2],vec![7,8],vec![4,5]]), 3);
    }
}

```
