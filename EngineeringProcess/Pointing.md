# Pointing

[Source](https://www.linkedin.com/pulse/sprint-work-pointing-estimation-arthur-messal-qypnc/?trackingId=3JkBjSraTBa%2Bb77AHGirxw%3D%3D) - Arthur Messal
 
Note: I wrote the first version of this years ago for anyone to use and have used it very successfully with many teams. This is the latest and somewhat simplified iteration. Feedback is always welcome.

## Context
In order to better understand the amount of work we can do we need to estimate the work we are planning to do. Using time to estimate presents some challenges because it’s nearly impossible to estimate with that level of accuracy. Instead, we will use Story Points, or just, Points.

## Points
Three factors contribute to the number of Points an issue receives. Points generally increase as each factor becomes more significant.

## Complexity
Level of Effort, how much work?
Risk, uncertainty, ambiguity



1 - Extra Small: generally a trivial and well understood task Example: A one line change requiring only local testing and deployment.

2 - Small: straightforward but not trivial with some complexity or minor ambiguity. Example: A one line change to a YAML file requiring a sanity check in pre-prod then deployment. Adding a feature flag.

3 - Medium: A task where focus/flow is needed, some element of complexity or ambiguity is present. Example: Writing a simple new API endpoint that is very similar to an existing one, including test coverage and deployment.

5 - Large: This is generally a task that is either quite complex, quite ambiguous, or simply a lot of work but generally only one of these factors. Example: Implementing a new service level function that performs a complex DB query but requires minimal scaffolding to test, including test coverage and deployment.

8 - Extra Large: Like large but with multiple factors and likely the need for some level of design, whiteboarding, and collaboration to resolve. Example: Modifying the schema for an existing object/table to add a column, refactoring all affected code. Implementing the service layer functions to support the new intended functionality, test coverage and deployment.

13 - Needs to be broken up, the number is not indicative of the scope but of the uncertainty and need for additional work to properly scope the task, often a tech design is needed to do this.



# Axioms
“Everything should be pointed”

“Everything should have a ticket”



# FAQ
Q: I’m learning a new technology/tool/language... should the time I spend on this have a ticket and be pointed?

A: No. Only work that contributes to the velocity of the team. The cost to come up to speed on new tech is an understood inefficiency and may slow down velocity.



Q: I’m working on a POC as the first step in a larger initiative. Should this work have a ticket and be pointed?

A: Yes! Since this is directly part of executing on a larger initiative which is part of team velocity



Q: When should I re-point a ticket?

A: If the original pointing was simply incorrect, we should repoint. For example, if we thought a task was “straightforward, but not trivial” (2), then later realize that it will actually require significant coding where focus/flow is needed, then we should repoint to a 3. When we do repoint, we should consider how we can improve our original pointing going forward - it should be a learning experience and we should get better!



Q: When should you not re-point?

A: If the task task is longer than anticipated due to something besides complexity, level of effort required, or risk/certainty/ambiguity. For example, if the engineer is new to this area and needs to do some self-learning along the way; or there were many interruptions that made progress on the task slow and inefficient.
