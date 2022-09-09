---
title: Leetcode 0489 robot room cleaner
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0489 robot room cleaner in Rust
---

## Solution in Rust

```rust
# """
# This is the robot's control interface.
# You should not implement it, or speculate about its implementation
# """
#class Robot:
#    def move(self):
#        """
#        Returns true if the cell in front is open and robot moves into the cell.
#        Returns false if the cell in front is blocked and robot stays in the current cell.
#        :rtype bool
#        """
#
#    def turnLeft(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def turnRight(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def clean(self):
#        """
#        Clean the current cell.
#        :rtype void
#        """

class Solution:
    def cleanRoom(self, robot):
        """
        :type robot: Robot
        :rtype: None
        """
        #stack = []
        def next_position(x, y, direction):
            if direction == 0: # North
                new_x = x - 1
                new_y = y
            elif direction == 1: # East
                new_x = x
                new_y = y + 1
            elif direction == 2: # South
                new_x = x + 1
                new_y = y
            elif direction == 3: # West
                new_x = x
                new_y = y - 1
            return (new_x, new_y)

        def cleanRoomHelper(robot, state, visited, cleaned):
            (x, y, direction) = state
            (next_x, next_y) = next_position(x, y, direction)
            if not (next_x, next_y, direction) in visited:
                success = robot.move()
                if success:
                    visited.add((next_x, next_y, direction))
                    if not (next_x, next_y) in cleaned:
                        robot.clean()
                        cleaned.add((next_x, next_y))
                    cleanRoomHelper(robot, (next_x, next_y, direction), visited, cleaned)
                    # reset Robot back to origin state
                    robot.turnLeft()
                    robot.turnLeft()
                    robot.move()
                    robot.turnLeft()
                    robot.turnLeft()
            turn_left_state = (x, y, (direction + 4 - 1) % 4)
            if not turn_left_state in visited:
                visited.add(turn_left_state)
                robot.turnLeft()
                cleanRoomHelper(robot, turn_left_state, visited, cleaned)
                robot.turnRight() # return to initial state
            turn_right_state = (x, y, (direction + 1) % 4)
            if not turn_right_state in visited:
                visited.add(turn_right_state)
                robot.turnRight()
                cleanRoomHelper(robot, turn_right_state, visited, cleaned)
                robot.turnLeft() # return to initial state



        visited = set([(0, 0, 0)])
        cleaned = set([(0, 0)])
        robot.clean()
        cleanRoomHelper(robot, (0, 0, 0), visited, cleaned)
        return

```
