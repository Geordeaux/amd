---
id: dev-process
title: Development process in iOS infra
---

## Introduction

The dev process in iOS infra team is based on sprints execution.

**The purpose of having sprints execution:**

- Performance tracking - we can compare results of different sprints to understand what is overall team performance and what affects it
- Better resource distribution and utilization
- Better predictability of the project execution and completion
- More precise estimations
- Increase transparency, understand what each team member involved to, address blockers and provide help to other projects if required
- Help new comers to understand what we are doing as infra team
- Commitment and self-control - we can make sure we keep our own commitments

## Difference from scrum

**The difference between our development process and classic scrum:**

- There are multiple independent technical projects for the one team
- No stakeholder, no product owner, stakeholders are project owners
- No single sprint goal, goal enrichment is defined by specific projects

## Scrum master

We have dedicated scrum master role, this is just an administrative role that can be picked up by anyone and therefore should be transferable. This is not suppose to be the exclusive activity, one person can be both a scrum master and a developer in this process.

**Scrum master role responsibilities:**

- Lead sprint activities
- Check that developers follows the process and properly participate in sprint activities
- Organize meetings and calls
- Make sprint reports
- Help with projects prioritization, suggest to assign free resources to top priority projects

## Projects / Epics

The development in iOS infra team is usually related to specific project.

**Rules for project tracking in JIRA:**

- The project scope is defined by special JIRA issue - Epic, Epic == Project
- Each epic has an assignee - project owner
- Epic groups all tickets related to the project
- Each epic has specific timeline and ETA
- Each epic should have a meaningful description or link to a full description

**Rules related to epic life time:**

- Try to avoid "uncompletable" epics, which never ends and always used for adding new issues
- It is better to make an epic for quarter or epic for few quarters if the project is cross quarter
- For example create something like "CI Improvements for 2021Q1" and try to complete it within the given period
- Use grooming sessions to prepare epic scope, goals, timeline, etc

## Estimation

Minimal part of the work is related to regular JIRA issue.

**Rules for ticket estimation:**

- We use story points (SP) for issue estimation, story points approach is rather complexity based than time based
- We use Fibonacci scale for SP - 0, 1, 2, 3, 5, 8, 13
- We basically try to avoid non-scale estimations, usually the higher estimation is taken, e.g. if it's between 3 and 5 then better to assign 5 as final estimation
- Task decomposition is required if the issue is estimated in 13 and more SP - means, all tickets should be completable within 1 sprint
- We do not use subtasks for decomposition and estimation, we prefer to split bigger tasks to smaller tasks of the same rank
- Our usually the max capacity is 8-12 per person

**SP approximate relation to the development days:**

- 0 - 1-2 hours
- 1 - less then 1 day
- 2 - 1-2 days
- 3 - 2-3 days
- 5 - 3-5 days, half of sprint
- 8 - more then half of sprint, but less then whole sprint
- 13 - whole sprint and most likely more then one sprint (should be decomposed)

## Sprints

**Sprint rules:**

- Sprint starts on Monday and lasts 2 weeks
- Sprint name is current year plus current week number (e.g. `iOS infra 2021 W8`, 2021 year, week number 8)
- Scope of the sprint is not allowed to be changed if sprint is already started
- In case if we really need to add urgent tasks we must exclude the previously added tasks with equivalent amount of SP
- Sprint can include not only Epics, but also tasks with no epics, bugs and also tasks from different big projects (if needed)

**Activities included into the sprint:**

- Standup
- Grooming
- Sprint review / retrospective
- Planning

## Standup

**Notes:**

- This is daily offline virtual meeting that is conducted in the related group chat
- This should be your first activity when you start work, this is similar to saying "hi" to the team
- The purpose is to make sure that everything is on track and to help resolve blockers and address questions

**Checklist for developers:**

- Report current work status in group chat:
    - What was done yesterday
    - What is planned for today
    - Any blockers or question
- Open the JIRA board, update ticket status if it is not updated, close tasks which are already done
- Notify if there are any sprint scope changes - e.g. tasks are added or removed from sprint (usually this should not happen)
- Check if there's anything you can help with to other people in the sprint (questions / blockers)

**Checklist for scrum master:**

- Check that everyone sent the update
- Check that tickets on the board are up-to-date with reported status
- Help to resolve blockers or answer questions, help to find who's responsible for a specific blocker
- Check if there's any impact for the sprint scope, include this into the sprint report
- Check if anyone completed all tasks in the sprint and has nothing to do in the sprint, suggest to join other activities

## Grooming

**Notes:**

- This is offline virtual meeting which is usually conducted once in a week
- Online meeting can be organized upon request, if there's anything that involves the whole team into the discussion

**Goals:**

- Maintain the sprint backlog related to specific epics
- By the end of this activity there should be JIRA tickets prepared for picking up into the sprint

**Checklist for developers:**

- Check all your epics one by one, check status, ETA, description, create or close epics if needed
- Close tasks which are not needed anymore
- Add new tickets
- Improve tickets description
- Estimate or re-estimate existing tickets
- Decompose large tasks to smaller tasks
- Move tickets from backlog to upcoming sprint (which is created but not started yet)
- Discuss all the above with team and project participants if needed

**Checklist for scrum master:**

- Make sure backlog doesn't grow too much
- Check new tickets, make sure it is clear what should be done
- Ask to update description if something is not clear
- Check that the estimation follows the rules
- Check that ticket decomposition was properly done
- Suggest your help if needed

## Sprint review / retrospective

Sprint review and retrospective are unified into single activity.

**Goals:**

- Calculate the performance and understand if it is lower or higher than in previous sprint
- Understand what affects the performance - analyze and try to avoid anything that decrease performance and make more things that increase the performance in future
- Collect and address team feedback

**Checklist for developers (all should be done on Friday on the last day of the sprint):**

- Send sprint feedback
- Check that all tickets are up-to-date
- Check that all completed tasks are closed
- Adjust remaining estimation
- Important - do not move uncompleted tickets to the next sprint, this should be done after closing the sprint

**Checklist for scrum master:**

- On Friday (last day of sprint):
    - Remove all previous answers in the feedback form
    - Send the reminder to the group chat about what should be done
    - Remind to send a feedback through the feedback form (optional)
- On Monday before meeting:
    - Remind to update tickets once again before creating a sprint report
    - Prepare sprint review - check sprint reports, add burndown chart, add sprint highlights
    - To calculate completed/not completed SP use Sprint Report from Reports
    - Prepare sprint retrospective - check feedback form, add diagrams and move the feedback items to the form
    - Suggest actionable items for each item from "what should we do better", this will be reviewed during online meeting
    - Create next-next sprint to put the tasks which doesn't fit into the upcoming sprint, e.g. if W6 is closed, then W8 is next, so W10 should be created
- On Monday during the call:
    - Open the sprint board and check all uncompleted tickets in the sprint one by one
    - Close tickets if they are done, but not updated yet
    - Consider to move task to the next sprint if it's not completed, ask if any help is required
    - Do not move the task if sprint is not closed
    - Close the sprint after checking all tasks
    - Check the sprint velocity report in JIRA and add picture to the sprint report
    - Open sprint review document and share the results and sprint highlights
    - Move next to the retrospective feedback
    - Check Actionable items from previous retrospective
    - Check "how do we feel" diagrams, try understand why we don't feel well or why we do not feel productive
    - Read all feedback, ask for clarification if needed
    - Suggest actionable items, confirm with team, transform actionable items into JIRA tasks if needed

## Sprint planning

This activity takes the same meeting slot with sprint review / retrospective. They unified into one online meeting for sprint review, retrospective and sprint planning.

High level goal is to prepare the sprint for execution and start the sprint.

**Checklist for developers:**

- Prepare the list of tickets for the next sprint for yourself and for all your project and project assignees
- Notify scrum master if more time is required, sprint start could be postponed until the end of the day
- Make sure your project assignees have enough tasks for execution, but tasks don't exceed the capacity
- There should be around 8-12 SP per person per sprint

**Checklist for scrum master:**

- On Friday - collect the planned absences / vacation / holiday and calculate the sprint capacity
- Check with everyone that all required tasks are added to the sprint
- Calculate SP for the current sprint and compare with capacity
- Check if everyone has enough tasks, but don't exceed the capacity
- If someone was not completed the all planned tickets in previous sprint with given capacity - suggest to take less tasks next sprint
- Start the sprint if everything is OK, otherwise wait by the end of the day to let everyone complete the planning
- Do not change the sprint scope if sprint is already started

## JIRA reference

**JIRA boards - Backlog:**

![Backlog](https://static.devfdg.net/static/mono-static/docs-ui/img/backlog.jpg)

- 1 - Backlog mode of the scrum board, you can switch between backlog and active sprints mode
- 2 - Quick filters, text search on the left side can be also used
- 3 - Detailed info of the selected issue
- 4 - Epics panel, filters issues by epic if epic is selected
- 5 - Active sprint, sprint name, count of issues in the sprint and time interval
- 6 - Selected issue, epic is shown on the right side (underlined)
- 7 - Count of story points in the sprint, total, in progress, done
- 8 - Estimation in story points, should be set here
- 9 - Current estimation in SP for an issue
- 10 - Labels, `ios-infra` label makes everything visible on this board

**JIRA boards - Active sprints:**

![Board](https://static.devfdg.net/static/mono-static/docs-ui/img/board.jpg)

- 1 - Active sprint mode of the scrum board, you can switch between backlog and active sprints mode
- 2 - Board columns, represent current status of the issue
- 3 - Issue card, ID, title, Epic name, estimation in SP
- 4 - Drag and drop issues to put them in appropriate status, there might be more statuses then column, you can put it to any status in the column
- 5 - Complete sprint, use this button to complete sprint during a sprint review

**Reports:**

![Reports](https://static.devfdg.net/static/mono-static/docs-ui/img/reports.jpg)

- 1 - Reports mode of the scrum board
- 2 - Current report type, you can switch to another type, the most interesting for us are Sprint Report and Velocity Chart
- 3 - Current sprint, you should select appropriate sprint when making the sprint review report
- 4 - Burndown chart, you can use specific report or dashboard to get a detailed view
- 5 - Story points of the completed issues group (the number of issues `at the beginning` -> `at the end` of the sprint)

**Dashboard:**

![Dashboard](https://static.devfdg.net/static/mono-static/docs-ui/img/dashboard.jpg)

- 1 - You can find the dashboard in Dashboards menu
- 2 - Days left
- 3 - Active sprint burndown chart
- 4 - Sprint health, how much time left, how much is completed, scope change
- 5 - You can find other useful dashboard gadgets below

## References

- iOS infra v2 board - <https://jira.toolsfdg.net/secure/RapidBoard.jspa?rapidView=611>
- iOS infra v2 dashboard - https://jira.toolsfdg.net/secure/Dashboard.jspa?selectPageId=10751
- Sprint reports and retrospectives doc - <https://docs.google.com/document/d/1DhpOS30Mj8DgqujpeSdch3G5FPYp3b3mGgAXO9VQx_E/edit>
- Feedback form - <https://forms.gle/rEJiKpUxoRyJKKC77>
