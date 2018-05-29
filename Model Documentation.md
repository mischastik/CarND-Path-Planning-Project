# Starting Point
The initial version incorporates all of the methods suggested in the walk-through:
* The path points are generated with spline interpolation (also for lane change maneuvers).
* Unused points from previous planning sessions are re-used.
* The target velocity is set to 49.5 MPH.
* Lane changes are made when possible and reasonable.

# Lane Choice Strategy
The path planning part is not explicitly modeled as a state machine because the rule-set is very small.
Instead, a simple decision hierachy is modeled with two kinds of decisions: 
* Absolute decisions: First rule out any maneuvers that are impossible or unsafe.
* Score-based: If there is more than one option left, try to take the better one.

The decisions are designed as follows:
1. Check if my lane is free (no car less than 30 meters ahead):
   1. Yes: Stay there **PLANING ITERATION FINISHED**
   1. No: Check status of neighbor lanes.
1. Neighbor lane is blocked if a car is 30 meters ahead or 10 meters behind or alongside ego vehicle or if it not a valid lane.
1. If only one neighbor lane is free: Switch lanes to free lane. **PLANING ITERATION FINISHED**
1. If no neighbor lane is free: Stay in current lane and decelerate **PLANING ITERATION FINISHED**
1. If both neighbor lanes are free: Score lanes and switch to the one with the closest car ahead that's futher away. **PLANING ITERATION FINISHED**

# Pros and Cons of this Model
## Cons
* Only neighboring lines are considered for maneauver planning.
* Very limited mid- to long-term panning capabilities.
* Inappropriate for more complicated rule sets.
## Pros
* Reactes very quickly to dynamic changes in the driving situations (e. g. when a lane gets free).
* Is rather fast, speed is only reduced when all other options are unsafe or impossable.
* Minimizes number of maneuvers to an acceptable extent.
* Manages to avoid any violations in several runs of the test scenario.
