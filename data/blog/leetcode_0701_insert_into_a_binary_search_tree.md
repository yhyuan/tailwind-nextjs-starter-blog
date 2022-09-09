---
title: Leetcode 0701 insert into a binary search tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0701 insert into a binary search tree in Rust
---

## Solution in Rust

```rust
/**
 * [701] Insert into a Binary Search Tree
 *
 * You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.
 * Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg" style="width: 752px; height: 221px;" />
 * Input: root = [4,2,7,1,3], val = 5
 * Output: [4,2,7,1,3,5]
 * Explanation: Another accepted tree is:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/05/bst.jpg" style="width: 352px; height: 301px;" />
 *
 * Example 2:
 *
 * Input: root = [40,20,60,10,30,50,70], val = 25
 * Output: [40,20,60,10,30,50,70,null,null,25]
 *
 * Example 3:
 *
 * Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
 * Output: [4,2,7,1,3,5]
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree will be in the range [0, 10^4].
 * 	-10^8 <= Node.val <= 10^8
 * 	All the values Node.val are unique.
 * 	-10^8 <= val <= 10^8
 * 	It's guaranteed that val does not exist in the original BST.
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/insert-into-a-binary-search-tree/
// discuss: https://leetcode.com/problems/insert-into-a-binary-search-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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

type TreeLink = Option<Rc<RefCell<TreeNode>>>;

trait Insert {
    fn insert(self, val: i32) -> TreeLink;
}
impl Insert for TreeLink {
    fn insert(self, val: i32) -> TreeLink {
        if let Some(node) = self {
            let node_val = node.borrow().val;
            let left = node.borrow_mut().left.take();
            let right = node.borrow_mut().right.take();
            if val < node_val {
                Some(Rc::new(RefCell::new(TreeNode {
                    val: node_val,
                    left: left.insert(val),
                    right,
                })))
            } else {
                Some(Rc::new(RefCell::new(TreeNode {
                    val: node_val,
                    left,
                    right: right.insert(val),
                })))
            }
        } else {
            Some(Rc::new(RefCell::new(TreeNode::new(val))))
        }
    }
}
impl Solution {
    pub fn insert_into_bst(root: Option<Rc<RefCell<TreeNode>>>, val: i32) -> Option<Rc<RefCell<TreeNode>>> {
        let mut root = root;
        root.insert(val)
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_701() {
    }
}

```
