---
title: Leetcode 0653 two sum iv input is a bst
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0653 two sum iv input is a bst in Rust
---

## Solution in Rust

```rust
/**
 * [653] Two Sum IV - Input is a BST
 *
 * Given the root of a Binary Search Tree and a target number k, return true if there exist two elements in the BST such that their sum is equal to the given target.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg" style="width: 562px; height: 322px;" />
 * Input: root = [5,3,6,2,4,null,7], k = 9
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg" style="width: 562px; height: 322px;" />
 * Input: root = [5,3,6,2,4,null,7], k = 28
 * Output: false
 *
 * Example 3:
 *
 * Input: root = [2,1,3], k = 4
 * Output: true
 *
 * Example 4:
 *
 * Input: root = [2,1,3], k = 1
 * Output: false
 *
 * Example 5:
 *
 * Input: root = [2,1,3], k = 3
 * Output: true
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [1, 10^4].
 * 	-10^4 <= Node.val <= 10^4
 * 	root is guaranteed to be a valid binary search tree.
 * 	-10^5 <= k <= 10^5
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/two-sum-iv-input-is-a-bst/
// discuss: https://leetcode.com/problems/two-sum-iv-input-is-a-bst/discuss/?currentPage=1&orderBy=most_votes&query=

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
use std::collections::HashSet;
type TreeLink = Option<Rc<RefCell<TreeNode>>>;
trait Find {
    fn find(&self, hashset: &mut HashSet<i32>, k: i32) -> bool;
}
impl Find for TreeLink {
    fn find(&self, hashset: &mut HashSet<i32>, k: i32) -> bool {
        if let Some(node) = self {
            let _node = node.borrow();
            if hashset.contains(&_node.val) {
                true
            } else {
                hashset.insert(k - _node.val);
                _node.left.find(hashset, k) || _node.right.find(hashset, k)
            }
        } else {
            false
        }
    }
}
impl Solution {
    pub fn find_target(root: Option<Rc<RefCell<TreeNode>>>, k: i32) -> bool {
        let mut hashset: HashSet<i32> = HashSet::new();
        root.find(&mut hashset, k)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_653() {
    }
}

```
