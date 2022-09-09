---
title: Leetcode 0150 evaluate reverse polish notation
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0150 evaluate reverse polish notation in Rust
---

## Solution in Rust

```rust
/**
 * [150] Evaluate Reverse Polish Notation
 *
 * Evaluate the value of an arithmetic expression in <a href="http://en.wikipedia.org/wiki/Reverse_Polish_notation" target="_blank">Reverse Polish Notation</a>.
 * Valid operators are +, -, *, and /. Each operand may be an integer or another expression.
 * Note that division between two integers should truncate toward zero.
 * It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.
 *
 * Example 1:
 *
 * Input: tokens = ["2","1","+","3","*"]
 * Output: 9
 * Explanation: ((2 + 1) * 3) = 9
 *
 * Example 2:
 *
 * Input: tokens = ["4","13","5","/","+"]
 * Output: 6
 * Explanation: (4 + (13 / 5)) = 6
 *
 * Example 3:
 *
 * Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
 * Output: 22
 * Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
 * = ((10 * (6 / (12 * -11))) + 17) + 5
 * = ((10 * (6 / -132)) + 17) + 5
 * = ((10 * 0) + 17) + 5
 * = (0 + 17) + 5
 * = 17 + 5
 * = 22
 *
 *
 * Constraints:
 *
 * 	1 <= tokens.length <= 10^4
 * 	tokens[i] is either an operator: "+", "-", "*", or "/", or an integer in the range [-200, 200].
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/evaluate-reverse-polish-notation/
// discuss: https://leetcode.com/problems/evaluate-reverse-polish-notation/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn eval_rpn(tokens: Vec<String>) -> i32 {
        let mut stack: Vec<i32> = vec![];
        for token in tokens {
            if token.len() == 1 {
                let chars: Vec<char> = token.chars().collect();
                if chars[0] == '+' || chars[0] == '-' || chars[0] == '*' || chars[0] == '/' {
                    let v1 = stack.pop().unwrap();
                    let v2 = stack.pop().unwrap();
                    let v = match chars[0] {
                        '+' => {
                            v1 + v2
                        },
                        '-' => {
                            v2 - v1
                        },
                        '*' => {
                            v1 * v2
                        },
                        '/' => {
                            v2 / v1
                        }
                        _ => {
                            panic!("wrong input")
                        }
                    };
                    stack.push(v);
                } else {
                    stack.push(token.parse::<i32>().unwrap());
                }
            } else {
                stack.push(token.parse::<i32>().unwrap());
            }
        }
        stack.pop().unwrap()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_150() {
        assert_eq!(Solution::eval_rpn(vec!["10".to_string(),"6".to_string(),"9".to_string(),"3".to_string(),"+".to_string(),"-11".to_string(),"*".to_string(),"/".to_string(),"*".to_string(),"17".to_string(),"+".to_string(),"5".to_string(),"+".to_string()]), 22);
    }
}

```
