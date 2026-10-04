# Feature Specification: Habit Tracker with Streaks and Reminders

**Feature Branch**: `001-habit-tracker`
**Created**: 2026-10-04
**Status**: Draft
**Input**: User description: "Build a habit tracker web app with streaks and daily reminders"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Create and check off daily habits (Priority: P1)

A user creates a few habits (e.g. "Read 20 minutes") and marks each one done for today. This alone is a usable tracker.

**Why this priority**: Core value; every other feature depends on habits and completions existing.

**Independent Test**: Create two habits, mark one done today; the list shows one done and one pending, and the state survives a page reload.

**Acceptance Scenarios**:

1. **Given** no habits exist, **When** the user adds "Read 20 minutes", **Then** it appears in today's list as not done.
2. **Given** a habit is not done today, **When** the user marks it done, **Then** it shows as done and can be un-marked.
3. **Given** a habit is done today, **When** the user returns the next day, **Then** it appears as not done for the new day.

---

### User Story 2 - See streaks (Priority: P2)

A user sees, per habit, the current streak (consecutive days completed) and their longest streak.

**Why this priority**: Streaks are the motivation mechanism that differentiates this from a plain to-do list.

**Independent Test**: With a habit completed on 3 consecutive days, the current streak shows 3; skipping a day resets it to 0 while the longest streak stays 3.

**Acceptance Scenarios**:

1. **Given** a habit completed yesterday and today, **When** the user views it, **Then** the current streak is 2.
2. **Given** a habit missed yesterday, **When** the user completes it today, **Then** the current streak is 1.

---

### User Story 3 - Daily reminders (Priority: P3)

A user chooses a reminder time per habit and is reminded if the habit is not yet done.

**Why this priority**: Improves adherence but needs a delivery channel decision, so it follows the core loop.

**Independent Test**: Set a reminder for a time a few minutes ahead on an incomplete habit; a reminder is delivered at that time, and none is delivered if the habit is already done.

**Acceptance Scenarios**:

1. **Given** a habit with a 8:00 PM reminder that is not done, **When** it is 8:00 PM, **Then** the user receives a reminder.
2. **Given** the same habit already done today, **When** it is 8:00 PM, **Then** no reminder is sent.

---

### Edge Cases

- What happens when the user changes time zone or the device clock crosses midnight?
- How does the system handle completing a habit for a past day (backfilling)?
- What happens to streaks when a habit is deleted and re-created with the same name?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Users MUST be able to create, rename, and delete habits.
- **FR-002**: Users MUST be able to mark a habit done or not done for the current day.
- **FR-003**: System MUST persist habits and completion history between sessions.
- **FR-004**: System MUST show current and longest streak per habit, counted in consecutive calendar days.
- **FR-005**: Users MUST be able to set an optional daily reminder time per habit.
- **FR-006**: System MUST skip the reminder when the habit is already done that day.
- **FR-007**: System MUST deliver reminders via [NEEDS CLARIFICATION: channel not specified - browser notification, email, or both?]
- **FR-008**: System MUST support [NEEDS CLARIFICATION: single user on one device, or accounts with sync across devices?]

### Key Entities

- **Habit**: a named recurring activity with an optional reminder time.
- **Completion**: a record that a habit was done on a specific calendar day.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A new user can create a habit and mark it done in under 30 seconds.
- **SC-002**: Streak values shown always match the underlying completion history (0 discrepancies in testing).
- **SC-003**: At least 95% of reminders are delivered within 1 minute of the chosen time.

## Assumptions

- Day boundaries use the user's local time zone.
- Mobile-native apps are out of scope for v1; a responsive web app is sufficient.
- Backfilling past days is out of scope for v1 unless clarified.
