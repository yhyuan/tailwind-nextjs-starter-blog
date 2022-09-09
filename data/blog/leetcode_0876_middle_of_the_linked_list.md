---
title: Leetcode 0876 middle of the linked list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0876 middle of the linked list in Rust
---

## Solution in Rust

```rust
/**
 * [876] Middle of the Linked List
 *
 * Given the head of a singly linked list, return the middle node of the linked list.
 * If there are two middle nodes, return the second middle node.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg" style="width: 544px; height: 65px;" />
 * Input: head = [1,2,3,4,5]
 * Output: [3,4,5]
 * Explanation: The middle node of the list is node 3.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg" style="width: 664px; height: 65px;" />
 * Input: head = [1,2,3,4,5,6]
 * Output: [4,5,6]
 * Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is in the range [1, 100].
 * 	1 <= Node.val <= 100
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/middle-of-the-linked-list/
// discuss: https://leetcode.com/problems/middle-of-the-linked-list/discuss/?currentPage=1&orderBy=most_votes&query=

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
impl Solution {
    pub fn middle_node(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut p = head.as_ref();
        let mut count = 0;
        while p.is_some() {
            count += 1;
            p = p.unwrap().next.as_ref();
        }
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        let mut p = head.as_ref();
        let mut i = 0;
        while p.is_some() {
            if i >= count / 2 {
                //return p;
                tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(p.unwrap().val)));
                tail = &mut tail.as_mut().unwrap().next;
            }
            i += 1;
            p = p.unwrap().next.as_ref();
        }
        dummy_head.unwrap().next
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_876() {
        assert_eq!(Solution::middle_node(linked![1,2,3,4,5]), linked![3,4,5]);
        assert_eq!(Solution::middle_node(linked![1,2,3,4,5,6]), linked![4,5,6]);
    }
}

```
