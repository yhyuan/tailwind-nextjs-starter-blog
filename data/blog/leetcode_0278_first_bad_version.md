---
title: Leetcode 0278 first bad version
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0278 first bad version in Rust
---

## Solution in Rust

```rust
/**
 * [278] First Bad Version
 *
 * You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.
 * Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.
 * You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.
 *
 * Example 1:
 *
 * Input: n = 5, bad = 4
 * Output: 4
 * Explanation:
 * call isBadVersion(3) -> false
 * call isBadVersion(5) -> true
 * call isBadVersion(4) -> true
 * Then 4 is the first bad version.
 *
 * Example 2:
 *
 * Input: n = 1, bad = 1
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	1 <= bad <= n <= 2^31 - 1
 *
 */
pub struct Solution {
  pub bad_version: i32,
}

// problem: https://leetcode.com/problems/first-bad-version/
// discuss: https://leetcode.com/problems/first-bad-version/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

// The API isBadVersion is defined for you.
// isBadVersion(versions:i32)-> bool;
// to call it use self.isBadVersion(versions)

impl Solution {
    pub fn new(bad_version: i32) -> Self {
      Solution {bad_version: bad_version}
    }
    pub fn isBadVersion(&self, n: i32) -> bool {
      self.bad_version <= n
    }
    pub fn first_bad_version(&self, n: i32) -> i32 {
      if self.isBadVersion(0) {
        return 0;
      }
      let mut low = 0;
      let mut high = n;
      while low + 1 < high {
        let mid = low + (high - low) / 2;
        if self.isBadVersion(mid) {
          high = mid;
        } else {
          low = mid
        }
      }
      if self.isBadVersion(low) {
        return low;
      }
      high
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use crate::solution;

    use super::*;

    #[test]
    fn test_278() {
      assert_eq!( Solution::new(4).first_bad_version(5), 4);
    }
}

```
