---
title: Leetcode 0199 binary tree right side view
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0199 binary tree right side view in Rust
---

## Solution in Rust

```rust
/**
 * [199] Binary Tree Right Side View
 *
 * Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/14/tree.jpg" style="width: 401px; height: 301px;" />
 * Input: root = [1,2,3,null,5,null,4]
 * Output: [1,3,4]
 *
 * Example 2:
 *
 * Input: root = [1,null,3]
 * Output: [1,3]
 *
 * Example 3:
 *
 * Input: root = []
 * Output: []
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [0, 100].
 * 	-100 <= Node.val <= 100
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/binary-tree-right-side-view/
// discuss: https://leetcode.com/problems/binary-tree-right-side-view/discuss/?currentPage=1&orderBy=most_votes&query=

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
use std::cell::{RefCell, Ref};
type TreeLink = Option<Rc<RefCell<TreeNode>>>;
trait Inorder {
    fn inorder(&self, visit: &mut dyn FnMut(i32, usize), layer: usize);
}
impl Inorder for TreeLink {
    fn inorder(&self, visit: &mut dyn FnMut(i32, usize), layer: usize) {
        if let Some(node) = self {
            let node: Ref<TreeNode> = node.borrow();
            node.left.inorder(visit, layer + 1);
            visit(node.val, layer);
            node.right.inorder(visit, layer + 1);
        }
    }
}
impl Solution {
    pub fn right_side_view(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut res:Vec<Vec<i32>> = vec![];
        root.inorder(&mut |x, layer| {
            if res.len() < layer + 1 {
                for i in res.len()..layer + 1 {
                    res.push(vec![]);
                }
            }
            res[layer].push(x);
        }, 0);
        res.into_iter().map(|nums| nums[nums.len() - 1]).collect()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_199() {
        assert_eq!(Solution::right_side_view(tree![1,2,3,null,5,null,4]), vec![1,3,4]);
        assert_eq!(Solution::right_side_view(tree![1,null,3]), vec![1,3]);
        //assert_eq!(Solution::right_side_view(tree![]), );
    }
}

```
