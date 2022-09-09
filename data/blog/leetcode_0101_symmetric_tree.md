---
title: Leetcode 0101 symmetric tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0101 symmetric tree in Rust
---

## Solution in Rust

```rust
/**
 * [101] Symmetric Tree
 *
 * Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg" style="width: 354px; height: 291px;" />
 * Input: root = [1,2,2,3,4,4,3]
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg" style="width: 308px; height: 258px;" />
 * Input: root = [1,2,2,null,3,null,3]
 * Output: false
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [1, 1000].
 * 	-100 <= Node.val <= 100
 *
 *
 * Follow up: Could you solve it both recursively and iteratively?
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/symmetric-tree/
// discuss: https://leetcode.com/problems/symmetric-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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
use std::cell::RefCell;
impl Solution {
    pub fn symmetric_helper(left: Option<Rc<RefCell<TreeNode>>>, right: Option<Rc<RefCell<TreeNode>>>) -> bool {
        match (left, right) {
            (Some(left), Some(right)) => {
                left.borrow().val == right.borrow().val
                && Solution::symmetric_helper(left.borrow().left.clone(), right.borrow().right.clone())
                && Solution::symmetric_helper(left.borrow().right.clone(), right.borrow().left.clone())
            }
            (None, None) => {
                true
            }
            _ => {
                false
            }
        }
    }

    pub fn is_symmetric(root: Option<Rc<RefCell<TreeNode>>>) -> bool {
        Solution::symmetric_helper(
             root.as_ref().and_then(|v| v.borrow().left.clone()),
            root.as_ref().and_then(|v| v.borrow().right.clone()))
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_101() {
        assert_eq!(Solution::is_symmetric(tree![1, 2, 2, 3, 4, 4, 3]), true);
        assert_eq!(
            Solution::is_symmetric(tree![1, 2, 2, null, 3, null, 3]),
            false
        );
        assert_eq!(Solution::is_symmetric(tree![]), true);
    }
}

```
