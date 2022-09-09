---
title: Leetcode 0234 palindrome linked list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0234 palindrome linked list in Rust
---

## Solution in Rust

```rust
/**
 * [234] Palindrome Linked List
 *
 * Given the head of a singly linked list, return true if it is a palindrome.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg" style="width: 422px; height: 62px;" />
 * Input: head = [1,2,2,1]
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg" style="width: 182px; height: 62px;" />
 * Input: head = [1,2]
 * Output: false
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is in the range [1, 10^5].
 * 	0 <= Node.val <= 9
 *
 *
 * Follow up: Could you do it in O(n) time and O(1) space?
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/palindrome-linked-list/
// discuss: https://leetcode.com/problems/palindrome-linked-list/discuss/?currentPage=1&orderBy=most_votes&query=

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
    pub fn is_palindrome(head: Option<Box<ListNode>>) -> bool {
        let mut p = head.as_ref();
        let mut nums: Vec<i32> = vec![];
        while p.is_some() {
            nums.push(p.unwrap().val);
            p = p.unwrap().next.as_ref();
        }
        for i in 0..nums.len() / 2 {
            if nums[i] != nums[nums.len() - 1 - i] {
                return false;
            }
        }
        true
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_234() {
        assert_eq!(Solution::is_palindrome(linked![1,2,2,1]), true);
        assert_eq!(Solution::is_palindrome(linked![1,2]), false);
    }
}

```
