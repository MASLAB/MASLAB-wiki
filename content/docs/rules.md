---
title: "Competition Rules"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
# bookIcon: ''
---

# Competition Rules

This entry contains information regarding the specifics of competition. This document should serve as a foundation for strategy formulation.

The content here content here is subject to change over the course of the class. If at any time there are questions regarding the rules, please reach out to a member of staff for clarification. 

## The Robot

### Construction

Your robot's dimensions must not exceed 12 inches in either of the horizontal directions directions, and must fit within the 1 sqft starting box. Your robot and its components must abide by [MIT EHS](https://ehs.mit.edu) guidelines; this entails restrictions on the following, but not limited to:

- lasers
- flammables and liquids
- explosives
- asbestos
- voltages exceeding 60V

The robot should be designed as and remain as a **single construction**. That is, your robot should not *eject* or *detach* any part of itself during the competition. 

### Interference

The robot may not be designed to intentionally damage any persons, other robots or the game field. No part of the robot that could be confused for game pieces or field parts (e.g., large red or green items) may cross the dividing border on the field, although parts of the robot may be so colored if they do not cross the border. The robot must not commit other egregious interference against the opposing robot, as determined by the judges.

### Autonomy

The robot may communicate over wireless networks (e.g. for telemetry or to offload computation), but must not be teleoperated during the game.

## The Game

### Field Layout

{{< figure src="/images/game_field.svg" width="100%" >}}

The playable area for each robot is a square (dimensions roughly 10 ft) defined by a blue tape border on the ground. Each team’s field will be semantically, but not geometrically symmetrical, and share a blue border between them. In addition to the blue borders, each field will have an area outlined in red, an area outlined in green, a small area outlined in yellow, and a predefined 17” x 17” starting area. 5 red, 5 green, and 1 golden MASLAB Calorie Enhancement Canisters will be placed on the center line in a randomized order. A distance of at least 3 cm will be ensured between each can.

### Gameplay

A match between two teams will consist of two rounds; the teams will switch sides between rounds. Each round is 2 minutes 30 seconds long. Both teams will have 5 minutes prior to the start of each round to prepare their robot on the field, after which the game will start regardless of whether or not teams are ready.

At the start of each round, a signal will be given by the judge. Both robots must then be activated with a single keystroke or button press, after which the robot must move autonomously. Any robots that begin moving early or do not show signs of life after the starting signal are deemed damaged (Section 3.b).

The winner of each match is the one who scores the most points summed across both
rounds.

### Scoring

At the end of the round, each robot will be carefully lifted upwards in a fully vertical direction. Cans that remain inside the robot will not be used for scoring.

- For each red can standing upright in the green zone, or green can standing upright in the red zone, +5 points
- For each red can standing upright in the red zone, or green can standing upright in the green zone, +10 points
- If the golden can is completely in the golden zone (no part is outside the yellow tape), the total score is multiplied by 2 and increased by 20.
- If cans are stacked, the number of points scored by those cans is multiplied by
the height of the stack. For example, if two red cans are stacked in the red zone, then the two cans collectively score 2 * 2 * 10 = 40 points

## Penalties

### Out of Bounds

Robots are considered to be “crossing the border” when the robot is completely over the tape. If the robot crosses any blue border (including the center line separating the teams’ fields), the team must:

1. Immediately stop all motors on the robot.
2. Wait for 30 seconds.
3. Place the robot back in the starting position.
4. Activate the robot again.

During the 30 second timeout, the team may fix any software or mechanical issues, as long as the robot does not enter the game arena (outside the starting position)

### Damaged robots

If the robot is damaged as a result of its own actions, the team must immediately stop all motors on the robot and fix the robot; the judge will determine if the robot has been satisfactorily fixed. After 30 seconds or the robot is fixed (whichever is later), the robot shall be placed back in the starting position and reactivated.

The judge and team members are given discretion to call a robot damaged. In addition to signs such as erratic behavior, power loss, or mechanical failure, any robot that has separated into at least two distinct pieces (even if intentional) is deemed damaged. The team is allowed to call their robot damaged and reset it (incurring the time penalty), even if the judge does not believe that the robot is damaged.

If the robot is damaged (either intentionally or unintentionally) by the opposing team, the opposing team will be disqualified for that match. The judge will make the final determination on whether the opposing robot is at fault. If both teams simultaneously damage each other, then the match will be fully reset and rerun.

### Human Interference

Human interference from team members is prohibited, and violators may be disqualified from the match. The judge may provide minimal assistance to allow robots to get unstuck if both robots are stuck along the dividing line

