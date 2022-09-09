---
title: Leetcode 0391 perfect rectangle
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0391 perfect rectangle in Rust
---

## Solution in Rust

```rust
/**
 * [391] Perfect Rectangle
 *
 * Given an array rectangles where rectangles[i] = [xi, yi, ai, bi] represents an axis-aligned rectangle. The bottom-left point of the rectangle is (xi, yi) and the top-right point of it is (ai, bi).
 * Return true if all the rectangles together form an exact cover of a rectangular region.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/27/perectrec1-plane.jpg" style="width: 300px; height: 294px;" />
 * Input: rectangles = [[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]
 * Output: true
 * Explanation: All 5 rectangles together form an exact cover of a rectangular region.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/27/perfectrec2-plane.jpg" style="width: 300px; height: 294px;" />
 * Input: rectangles = [[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]
 * Output: false
 * Explanation: Because there is a gap between the two rectangular regions.
 *
 * Example 3:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/27/perfectrec3-plane.jpg" style="width: 300px; height: 294px;" />
 * Input: rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[3,2,4,4]]
 * Output: false
 * Explanation: Because there is a gap in the top center.
 *
 * Example 4:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg" style="width: 300px; height: 294px;" />
 * Input: rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]
 * Output: false
 * Explanation: Because two of the rectangles overlap with each other.
 *
 *
 * Constraints:
 *
 * 	1 <= rectangles.length <= 2 * 10^4
 * 	rectangles[i].length == 4
 * 	-10^5 <= xi, yi, ai, bi <= 10^5
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/perfect-rectangle/
// discuss: https://leetcode.com/problems/perfect-rectangle/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_rectangle_cover(rectangles: Vec<Vec<i32>>) -> bool {
        false
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_391() {
    }
}

```
