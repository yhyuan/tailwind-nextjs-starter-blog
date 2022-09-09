---
title: Leetcode 0083 remove duplicates from sorted list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0083 remove duplicates from sorted list in Rust
---

## Solution in Rust

```rust
/**
 * [83] Remove Duplicates from Sorted List
 *
 * Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/list1.jpg" style="width: 302px; height: 242px;" />
 * Input: head = [1,1,2]
 * Output: [1,2]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/list2.jpg" style="width: 542px; height: 222px;" />
 * Input: head = [1,1,2,3,3]
 * Output: [1,2,3]
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is in the range [0, 300].
 * 	-100 <= Node.val <= 100
 * 	The list is guaranteed to be sorted in ascending order.
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/remove-duplicates-from-sorted-list/
// discuss: https://leetcode.com/problems/remove-duplicates-from-sorted-list/discuss/?currentPage=1&orderBy=most_votes&query=

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
    /**
    pub fn delete_duplicates(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        let mut head = head;
        let mut pre_val = i32::MIN;
        while let Some(node) = head {
            if node.val != pre_val {
                tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(node.val)));
                tail = &mut tail.as_mut().unwrap().next;
            }
            pre_val = node.val;
            head = node.next;
        }
        dummy_head.unwrap().next
    }
     */
    /*
    pub fn delete_duplicates(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        let mut p = head.as_ref();
        let mut pre_val = i32::MIN;
        while p.is_some() {
            let val = p.unwrap().val;
            if val != pre_val {
                tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(val)));
                tail = &mut tail.as_mut().unwrap().next;
            }
            pre_val = val;
            p = p.unwrap().next.as_ref();
        }
        dummy_head.unwrap().next
    }
    */
    pub fn delete_duplicates(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        let mut p = head.as_ref();
        let mut pre_val = i32::MIN;
        while p.is_some() {
            let val = p.unwrap().val;
            if val != pre_val {
                tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(val)));
                tail = &mut tail.as_mut().unwrap().next;
            }
            pre_val = val;
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
    fn test_83() {
        assert_eq!(Solution::delete_duplicates(linked![1,1,2,3,3]), linked![1, 2, 3]);
    }
}

```
