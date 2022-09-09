---
title: Leetcode 0572 subtree of another tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0572 subtree of another tree in Rust
---

## Solution in Rust

```rust
/**
 * [572] Subtree of Another Tree
 *
 * Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.
 * A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg" style="width: 532px; height: 400px;" />
 * Input: root = [3,4,5,1,2], subRoot = [4,1,2]
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg" style="width: 502px; height: 458px;" />
 * Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
 * Output: false
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the root tree is in the range [1, 2000].
 * 	The number of nodes in the subRoot tree is in the range [1, 1000].
 * 	-10^4 <= root.val <= 10^4
 * 	-10^4 <= subRoot.val <= 10^4
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/subtree-of-another-tree/
// discuss: https://leetcode.com/problems/subtree-of-another-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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
    pub fn is_subtree_helper(root: Option<&Rc<RefCell<TreeNode>>>, sub_root: Option<&Rc<RefCell<TreeNode>>>, search_mode: bool) -> bool {
        if root.is_none() && sub_root.is_none() {
            return true;
        }
        if root.is_none() && !sub_root.is_none() {
            return false;
        }
        if !root.is_none() && sub_root.is_none() {
            return false;
        }
        let node = root.unwrap().borrow();
        let val = node.val;
        let sub_root_node = sub_root.unwrap().borrow();
        let sub_val = sub_root_node.val;
        let left = node.left.as_ref();
        let right = node.right.as_ref();
        let sub_root_left = sub_root_node.left.as_ref();
        let sub_root_right = sub_root_node.right.as_ref();
        //println!("before val: {}, sub_val: {}", val, sub_val);
        if search_mode {
            return val == sub_val && Self::is_subtree_helper(left, sub_root_left, true)
                                    && Self::is_subtree_helper(right, sub_root_right, true)
        }
        if Self::is_subtree_helper(left, sub_root, false) || Self::is_subtree_helper(right, sub_root, false) {
            return true;
        }
        val == sub_val && Self::is_subtree_helper(left, sub_root_left, true) && Self::is_subtree_helper(right, sub_root_right, true)
    }
    pub fn is_subtree(root: Option<Rc<RefCell<TreeNode>>>, sub_root: Option<Rc<RefCell<TreeNode>>>) -> bool {
        Self::is_subtree_helper(root.as_ref(), sub_root.as_ref(), false)
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_572() {
        assert_eq!(Solution::is_subtree(tree![3,4,5,1,null,2], tree![3, 1, 2]), false);
        assert_eq!(Solution::is_subtree(tree![1,1], tree![1]), true);
    }
}

```
