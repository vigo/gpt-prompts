## Issue Description

We need a slack messaging functionality to track of what visitor does. In some
check points, We want to see what's visitor doing? This should be a Celery Task.

## Current Behavior

We don't have any solution, all we have is `SlackExceptionHandler` for errors

## Expected Behavior

We should be able to see slack messages (on slack) when user hits the following
views:

- `VisitorLoginView`
- `VisitorReadyView`
- `VisitorInterviewView`
- `VisitorLogoutView`
- `VisitorInterviewFinishedView`

## Proposed Solution

1. Examine `SlackExceptionHandler` code, how its communicating with Slack
1. Implement a function accepts message and calls the Slack
1. Examine other Celery tasks under `core/tasks` and understand the logic

## What are the Check Points?

- [ ] visitor logs in (registers) `VisitorLoginView`
- [ ] visitor hits ready `VisitorReadyView`
- [ ] visitor enters a quesiton `VisitorInterviewView`
- [ ] visitor quits / exists in the middle of interview `VisitorLogoutView`
- [ ] visitor completes interview `VisitorInterviewFinishedView`
- [ ] visitor logs out after interview complete `VisitorLogoutView`
- [ ] celery task fails

## Any Tips?

- in `core/utils/log.py` there is a `SlackExceptionHandler` you can check it and understand. 
- add your code `core/tasks/notifications.py`, name can be `send_message_to_slack`
