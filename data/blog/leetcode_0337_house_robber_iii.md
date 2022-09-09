---
title: Leetcode 0337 house robber iii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0337 house robber iii in Rust
---

## Solution in Rust

```rust
/**
 * [337] House Robber III
 *
 * The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.
 * Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.
 * Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg" style="width: 277px; height: 293px;" />
 * Input: root = [3,2,3,null,3,null,1]
 * Output: 7
 * Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg" style="width: 357px; height: 293px;" />
 * Input: root = [3,4,5,1,3,null,1]
 * Output: 9
 * Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [1, 10^4].
 * 	0 <= Node.val <= 10^4
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/house-robber-iii/
// discuss: https://leetcode.com/problems/house-robber-iii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::rc::Rc;
use std::cell::{RefCell};

impl Solution {
    pub fn rob(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        0
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_337() {
        assert_eq!(Solution::rob(tree![3,2,3,null,3,null,1]), 7);
        assert_eq!(Solution::rob(tree![3,4,5,1,3,null,1]), 9);
    }
}

```
