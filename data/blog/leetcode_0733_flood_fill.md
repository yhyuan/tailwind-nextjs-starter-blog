---
title: Leetcode 0733 flood fill
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0733 flood fill in Rust
---

## Solution in Rust

```rust
/**
 * [733] Flood Fill
 *
 * An image is represented by an m x n integer grid image where image[i][j] represents the pixel value of the image.
 * You are also given three integers sr, sc, and newColor. You should perform a flood fill on the image starting from the pixel image[sr][sc].
 * To perform a flood fill, consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with newColor.
 * Return the modified image after performing the flood fill.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg" style="width: 613px; height: 253px;" />
 * Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, newColor = 2
 * Output: [[2,2,2],[2,2,0],[2,0,1]]
 * Explanation: From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
 * Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.
 *
 * Example 2:
 *
 * Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, newColor = 2
 * Output: [[2,2,2],[2,2,2]]
 *
 *
 * Constraints:
 *
 * 	m == image.length
 * 	n == image[i].length
 * 	1 <= m, n <= 50
 * 	0 <= image[i][j], newColor < 2^16
 * 	0 <= sr < m
 * 	0 <= sc < n
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/flood-fill/
// discuss: https://leetcode.com/problems/flood-fill/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn flood_fill_helper(image: &mut Vec<Vec<i32>>, sr: i32, sc: i32, new_color: i32) {
        let old_color = image[sr as usize][sc as usize];
        image[sr as usize][sc as usize] = new_color;
        let rows = image.len();
        let cols = image[0].len();
        let diffs = vec![(-1, 0), (1, 0), (0, -1), (0, 1)];
        let candidates = diffs.iter()
            .map(|&(dx, dy)| (dx + sr, dy + sc))
            .filter(|&(x, y)| x >= 0 && y >=0 && x < rows as i32 && y < cols as i32)
            .filter(|&(x, y)| image[x as usize][y as usize] == old_color && image[x as usize][y as usize] != new_color)
            .collect::<Vec<_>>();
        for (i, &coor) in candidates.iter().enumerate() {
            Self::flood_fill_helper(image, coor.0, coor.1, new_color);
        }
    }
    pub fn flood_fill(image: Vec<Vec<i32>>, sr: i32, sc: i32, new_color: i32) -> Vec<Vec<i32>> {
        let mut image = image;
        Self::flood_fill_helper(&mut image, sr, sc, new_color);
        image
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_733() {
        assert_eq!(Solution::flood_fill(vec![vec![1,1,1],vec![1,1,0],vec![1,0,1]], 1, 1, 2), vec![vec![2,2,2],vec![2,2,0],vec![2,0,1]]);
    }
}

```
