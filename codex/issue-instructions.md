## Issue Description

We need a slack messaging functionality to track of what visitor does. In some check points, We want to see what's visitor doing?

## Current Behavior

We don't have any solution, all we have is `SlackExceptionHandler` for errors

## What are the Check Points?

- [ ] visitor logs in (registers) `VisitorLoginView`
- [ ] visitor hits ready `VisitorReadyView`
- [ ] visitor enters a quesiton `VisitorInterviewView`
- [ ] visitor quits / exists in the middle of interview `VisitorLogoutView`
- [ ] visitor completes interview `VisitorInterviewFinishedView`
- [ ] visitor logs out after interview complete `VisitorLogoutView`
- [ ] celery task fails

Tips:

- in `core/utils/log.py` there is a `SlackExceptionHandler` you can check it and understand. 
- add your code `core/tasks/notifications.py`, name can be `send_message_to_slack`

---

# Contributor Guide

## Dev Environment Tips
- Use pnpm dlx turbo run where <project_name> to jump to a package instead of scanning with ls.
- Run pnpm install --filter <project_name> to add the package to your workspace so Vite, ESLint, and TypeScript can see it.
- Use pnpm create vite@latest <project_name> -- --template react-ts to spin up a new React + Vite package with TypeScript checks ready.
- Check the name field inside each package's package.json to confirm the right name—skip the top-level one.

## Testing Instructions
- Find the CI plan in the .github/workflows folder.
- Run pnpm turbo run test --filter <project_name> to run every check defined for that package.
- From the package root you can just call pnpm test. The commit should pass all tests before you merge.
- To focus on one step, add the Vitest pattern: pnpm vitest run -t "<test name>".
- Fix any test or type errors until the whole suite is green.
- After moving files or changing imports, run pnpm lint --filter <project_name> to be sure ESLint and TypeScript rules still pass.
- Add or update tests for the code you change, even if nobody asked.

## PR instructions
Title format: [<project_name>] <Title>

---

### Task Description


### Current Behavior


### Expected Behavior


### Proposed Solution


### What are the Check Points?


### Any Tips?

---
