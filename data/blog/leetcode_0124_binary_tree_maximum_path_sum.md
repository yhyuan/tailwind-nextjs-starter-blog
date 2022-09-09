---
title: Leetcode 0124 binary tree maximum path sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0124 binary tree maximum path sum in Rust
---

## Solution in Rust

```rust
/**
 * [124] Binary Tree Maximum Path Sum
 *
 * A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.
 * The path sum of a path is the sum of the node's values in the path.
 * Given the root of a binary tree, return the maximum path sum of any path.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg" style="width: 322px; height: 182px;" />
 * Input: root = [1,2,3]
 * Output: 6
 * Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg" />
 * Input: root = [-10,9,20,null,null,15,7]
 * Output: 42
 * Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [1, 3 * 10^4].
 * 	-1000 <= Node.val <= 1000
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/binary-tree-maximum-path-sum/
// discuss: https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/?currentPage=1&orderBy=most_votes&query=

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
/*
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn max_path_sum(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut max = i32::min_value();
        Solution::postorder(root.as_ref(), &mut max);
        max
      }
      fn postorder(root: Option<&Rc<RefCell<TreeNode>>>, max: &mut i32) -> i32 {
        if let Some(node) = root {
            let left = Solution::postorder(node.borrow().left.as_ref(), max);
            let right = Solution::postorder(node.borrow().right.as_ref(), max);
            *max = i32::max(
                node.borrow().val + i32::max(left, 0) + i32::max(right, 0),
                *max,
            );
            node.borrow().val + i32::max(i32::max(left, right), 0)
        } else {
            0
        }
    }
}
*/
use std::rc::Rc;
use std::cell::{RefCell, Ref};
type TreeLink = Option<Rc<RefCell<TreeNode>>>;
trait Postorder {
    fn postorder(&self, visit: &mut dyn FnMut(i32, i32, i32)) -> i32;
}
impl Postorder for TreeLink {
    fn postorder(&self, visit: &mut dyn FnMut(i32, i32, i32)) -> i32 {
        if let Some(node) = self {
            let node: Ref<TreeNode> = node.borrow();
            let left = node.left.postorder(visit);
            let right = node.right.postorder(visit);
            visit(node.val, left, right);
            return node.val + i32::max(i32::max(left, right), 0);
        }
        0i32
    }
}
impl Solution {
    pub fn max_path_sum(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut max = i32::min_value();
        root.postorder(&mut |x, left, right| {
            max = i32::max(
                x + i32::max(left, 0) + i32::max(right, 0),
                max
            );
        });
        max
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_124() {
        assert_eq!(Solution::max_path_sum(tree![1, 2, 3]), 6);
        assert_eq!(
            Solution::max_path_sum(tree![-10, 9, 20, null, null, 15, 7]),
            42
        );
        assert_eq!(
            Solution::max_path_sum(tree![5, 4, 8, 11, null, 13, 4, 7, 2, null, null, null, 1]),
            48
        );
        assert_eq!(Solution::max_path_sum(tree![-3]), -3);
    }
}

```
