---
title: Leetcode 0203 remove linked list elements
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0203 remove linked list elements in Rust
---

## Solution in Rust

```rust
/**
 * [203] Remove Linked List Elements
 *
 * Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg" style="width: 500px; height: 142px;" />
 * Input: head = [1,2,6,3,4,5,6], val = 6
 * Output: [1,2,3,4,5]
 *
 * Example 2:
 *
 * Input: head = [], val = 1
 * Output: []
 *
 * Example 3:
 *
 * Input: head = [7,7,7,7], val = 7
 * Output: []
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is in the range [0, 10^4].
 * 	1 <= Node.val <= 50
 * 	0 <= val <= 50
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/remove-linked-list-elements/
// discuss: https://leetcode.com/problems/remove-linked-list-elements/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

// Definition for singly-linked list.
// #[derive(PartialEq, Eq, Clone, Debug)]
// pub struct ListNode {
//   pub val: i32,
//   pub next: Option<Box<ListNode>>
// }
//
// impl ListNode {
//   #[inline]
//   fn new(val: i32) -> Self {
//     ListNode {
//       next: None,
//       val
//     }
//   }
// }
/*
impl Solution {
    pub fn remove_elements(head: Option<Box<ListNode>>, val: i32) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        let mut p = head.as_ref();

        while p.is_some() {
            let value = p.unwrap().val;
            p = p.unwrap().next.as_ref();
            if value != val {
                tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(value)));
                tail = &mut tail.as_mut().unwrap().next;
            }
        }
        dummy_head.unwrap().next
    }
}
*/
impl Solution {
    pub fn remove_elements(head: Option<Box<ListNode>>, val: i32) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        let mut head = head;
        let mut p = head.take();
        while let Some(mut node) = p {
            //println!("val: {}", node.val);
            p = node.next.take();
            if node.val != val {
                tail.as_mut().unwrap().next = Some(node);
                tail = &mut tail.as_mut().unwrap().next;
            }
        }
        dummy_head.unwrap().next
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_203() {
        assert_eq!(Solution::remove_elements(linked![1,2,6,3,4,5,6], 6), linked![1,2,3,4,5]);
        assert_eq!(Solution::remove_elements(linked![], 6), linked![]);
        assert_eq!(Solution::remove_elements(linked![7,7,7,7], 7), linked![]);
    }
}

```
