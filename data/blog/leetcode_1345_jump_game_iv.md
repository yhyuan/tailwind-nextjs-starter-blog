---
title: Leetcode 1345 jump game iv
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1345 jump game iv in Rust
---

## Solution in Rust

```rust
/**
 * [1345] Jump Game IV
 *
 * Given an array of integers arr, you are initially positioned at the first index of the array.
 * In one step you can jump from index i to index:
 *
 * 	i + 1 where: i + 1 < arr.length.
 * 	i - 1 where: i - 1 >= 0.
 * 	j where: arr[i] == arr[j] and i != j.
 *
 * Return the minimum number of steps to reach the last index of the array.
 * Notice that you can not jump outside of the array at any time.
 *
 * Example 1:
 *
 * Input: arr = [100,-23,-23,404,100,23,23,23,3,404]
 * Output: 3
 * Explanation: You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.
 *
 * Example 2:
 *
 * Input: arr = [7]
 * Output: 0
 * Explanation: Start index is the last index. You don't need to jump.
 *
 * Example 3:
 *
 * Input: arr = [7,6,9,6,9,6,9,7]
 * Output: 1
 * Explanation: You can jump directly from index 0 to index 7 which is last index of the array.
 *
 * Example 4:
 *
 * Input: arr = [6,1,9]
 * Output: 2
 *
 * Example 5:
 *
 * Input: arr = [11,22,7,7,7,7,7,7,7,22,13]
 * Output: 3
 *
 *
 * Constraints:
 *
 * 	1 <= arr.length <= 5 * 10^4
 * 	-10^8 <= arr[i] <= 10^8
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/jump-game-iv/
// discuss: https://leetcode.com/problems/jump-game-iv/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
use std::collections::VecDeque;
impl Solution {
    pub fn min_jumps(arr: Vec<i32>) -> i32 {
        let n = arr.len();
        //println!("n: {}", n);
        let mut hashmap: HashMap<i32, Vec<usize>> = HashMap::new();
        for i in 0..n {
            let val = arr[i];
            if hashmap.contains_key(&val) {
                hashmap.get_mut(&val).unwrap().push(i);
            } else {
                hashmap.insert(val, vec![i]);
            }
        }
        //let mut dp: Vec<i32> = vec![-1; n];
        let mut visited: Vec<bool> = vec![false; n];
        let mut q: VecDeque<usize> = VecDeque::new();
        q.push_back(0);
        let mut step = 0;
        while !q.is_empty() {
            let size = q.len();
            //println!("size: {}", size);
            for _ in 0..size {
                let u = q.pop_front().unwrap();
                if u == n - 1 {
                    return step;
                }
                let val = arr[u];
                let indices = hashmap.get(&val).unwrap();
                for i in 0..indices.len() {
                    if indices[i] == u {
                        continue;
                    }
                    if visited[indices[i]] {
                        continue;
                    }
                    visited[indices[i]] = true;
                    q.push_back(indices[i]);
                }
                hashmap.insert(val, vec![]);
                if u + 1 < n && !visited[u + 1] {
                    visited[u + 1] = true;
                    q.push_back(u + 1);
                }
                if u >= 1 && !visited[u - 1]{
                    visited[u - 1] = true;
                    q.push_back(u - 1);
                }
            }
            step += 1;
        }
        -1
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1345() {
        assert_eq!(Solution::min_jumps(vec![100,-23,-23,404,100,23,23,23,3,404]), 3);
        assert_eq!(Solution::min_jumps(vec![7]), 0);
        assert_eq!(Solution::min_jumps(vec![7,6,9,6,9,6,9,7]), 1);
    }
}

```
