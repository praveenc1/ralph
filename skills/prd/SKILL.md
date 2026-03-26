---
name: prd
description: "Generate a Product Requirements Document (PRD) for a new feature, product, or scoped project increment. Use when planning a feature, starting a new project, defining requirements, scoping work, or when asked to create a PRD. Triggers on: create a prd, write prd for, plan this feature, requirements for, spec out, define product requirements, turn this idea into stories."
---

# PRD Generator

Create detailed Product Requirements Documents that are clear, actionable, and suitable for implementation.

---

## The Job

1. Receive a feature description from the user
2. Ask 3-5 essential clarifying questions (with lettered options)
3. Generate a structured PRD based on answers
4. Save to `tasks/prd-[feature-name].md`

**Important:** Do NOT start implementing. Just create the PRD.

**Goal:** Produce a PRD detailed enough that another AI coding agent can implement with little or no additional human input once brainstorming is complete.

---

## Definition of Ready for Autonomous Implementation

Treat the PRD as incomplete until it answers:

- What exact user problem is being solved
- Who the target users or operators are
- What concrete behaviors must exist
- What is explicitly out of scope
- What APIs, data changes, and events are required
- What security and permission constraints apply
- What tests prove the work is complete
- What order the work should happen in
- What can be built in parallel

If any of these are unclear, ask clarifying questions or record explicit assumptions in the PRD.

---

## Step 1: Clarifying Questions

Ask only critical questions where the initial prompt is ambiguous. Focus on:

- **Problem/Goal:** What problem does this solve?
- **Core Functionality:** What are the key actions?
- **Scope/Boundaries:** What should it NOT do?
- **Success Criteria:** How do we know it's done?
- **Users/Roles:** Who performs the action and who is affected?
- **Risk/Compliance:** Are there security, privacy, or audit implications?
- **Delivery Constraints:** Are there platform, deadline, or environment constraints?

### Format Questions Like This:

```
1. What is the primary goal of this feature?
   A. Improve user onboarding experience
   B. Increase user retention
   C. Reduce support burden
   D. Other: [please specify]

2. Who is the target user?
   A. New users only
   B. Existing users only
   C. All users
   D. Admin users only

3. What is the scope?
   A. Minimal viable version
   B. Full-featured implementation
   C. Just the backend/API
   D. Just the UI
```

This lets users respond with "1A, 2C, 3B" for quick iteration. Remember to indent the options.

If the request is already specific and the remaining ambiguity is low-risk, proceed and record assumptions instead of blocking.

---

## Step 2: PRD Structure

Generate the PRD with these sections:

### 1. Introduction/Overview
Brief description of the feature and the problem it solves.

### 2. Goals
Specific, measurable objectives (bullet list).

### 3. User Stories
Each story needs:
- **Title:** Short descriptive name
- **Description:** "As a [user], I want [feature] so that [benefit]"
- **Acceptance Criteria:** Verifiable checklist of what "done" means

Each story should be small enough to implement in one focused session.

**Format:**
```markdown
### US-001: [Title]
**Description:** As a [user], I want [feature] so that [benefit].

**Depends on:** US-000 (if applicable)

**Acceptance Criteria:**
- [ ] Specific verifiable criterion
- [ ] Another criterion
- [ ] Edge cases and empty states are defined
- [ ] Error handling is defined
- [ ] Security/permissions are defined when applicable
- [ ] API/schema/event implications are defined when applicable
- [ ] Unit tests added or updated
- [ ] Integration tests added or updated when applicable
- [ ] Typecheck/lint passes
- [ ] **[UI stories only]** Verify in browser using dev-browser skill
```

**Important:** 
- Acceptance criteria must be verifiable, not vague. "Works correctly" is bad. "Button shows confirmation dialog before deleting" is good.
- **For any story with UI changes:** Always include "Verify in browser using dev-browser skill" as acceptance criteria. This ensures visual verification of frontend work.
- Include edge cases, empty states, failure cases, and permission boundaries when they matter.
- Include enough detail that implementation sequencing is obvious.
- Prefer vertical slices or tightly scoped stories over large ambiguous stories.
- If a story mixes unrelated concerns, split it.

### Story Decomposition Guidance

To support autonomous coding:

- Keep stories independently testable
- Show prerequisites explicitly with `Depends on:`
- Separate backend, API, UI, auth, migration, analytics, and test work when doing so improves clarity
- Identify stories that can run in parallel
- Split stories again if a junior developer or AI agent would still need to ask basic follow-up questions

### 4. Functional Requirements
Numbered list of specific functionalities:
- "FR-1: The system must allow users to..."
- "FR-2: When a user clicks X, the system must..."

Be explicit and unambiguous.

### 5. Non-Goals (Out of Scope)
What this feature will NOT include. Critical for managing scope.

### 6. Design Considerations (Optional)
- UI/UX requirements
- Link to mockups if available
- Relevant existing components to reuse

### 7. Technical Considerations (Optional)
- Known constraints or dependencies
- Integration points with existing systems
- Performance requirements
- API-first implications before UI details when relevant
- Data model changes, migrations, or event contracts when relevant
- Architecture constraints that materially shape implementation
- Deployment/runtime constraints when relevant

### 8. Success Metrics
How will success be measured?
- "Reduce time to complete X by 50%"
- "Increase conversion rate by 10%"

### 9. Open Questions
Remaining questions or areas needing clarification.

---

## Writing for Junior Developers

The PRD reader may be a junior developer or AI agent. Therefore:

- Be explicit and unambiguous
- Avoid jargon or explain it
- Provide enough detail to understand purpose and core logic
- Number requirements for easy reference
- Use concrete examples where helpful

Write for execution, not inspiration. Prefer completeness over brevity when extra detail reduces implementation ambiguity.

---

## Security Requirements Guidance

When the feature touches authentication, authorization, user data, uploads, external integrations, admin tools, payments, or high-risk workflows, include security requirements explicitly.

Consider:

- Authentication and session expectations
- Authorization and least-privilege access
- Input validation and output encoding
- OWASP-style risks such as injection, XSS, CSRF, SSRF, broken access control, IDOR, and sensitive data exposure
- Audit logging and traceability
- Rate limiting and abuse prevention
- Secret handling
- Data retention, deletion, and privacy expectations

---

## API and Data Guidance

When relevant, define the API before the UI details.

Include:

- Endpoints or interface surfaces needed
- Request and response expectations
- Validation rules
- Error responses
- Data model or schema changes
- Event contracts for event-driven systems

---

## Delivery Planning Guidance

When the feature is non-trivial, include a delivery plan that breaks work into:

- Frontend
- Middle-tier / API
- Backend / data
- Testing

Also call out:

- Sequencing and dependency order
- Parallelizable workstreams
- Milestones such as MVP, hardening, and polish
- Ownership hints when multiple agents or roles can work in parallel

---

## PRD Quality Gate

Before saving the PRD, verify that it:

- Defines a concrete user problem and target user
- Contains measurable goals and clear non-goals
- Includes implementation-ready user stories
- Has unambiguous functional requirements
- Covers relevant non-functional and security requirements
- Defines API and data implications when relevant
- Identifies dependencies, sequencing, and parallelizable work
- Includes a testing strategy proportionate to the risk
- Is specific enough that an AI coding agent could execute without asking basic follow-up questions

---

## Output

- **Format:** Markdown (`.md`)
- **Location:** `tasks/`
- **Filename:** `prd-[feature-name].md` (kebab-case)

---

## Example PRD

```markdown
# PRD: Task Priority System

## Introduction

Add priority levels to tasks so users can focus on what matters most. Tasks can be marked as high, medium, or low priority, with visual indicators and filtering to help users manage their workload effectively.

## Goals

- Allow assigning priority (high/medium/low) to any task
- Provide clear visual differentiation between priority levels
- Enable filtering and sorting by priority
- Default new tasks to medium priority

## User Stories

### US-001: Add priority field to database
**Description:** As a developer, I need to store task priority so it persists across sessions.

**Acceptance Criteria:**
- [ ] Add priority column to tasks table: 'high' | 'medium' | 'low' (default 'medium')
- [ ] Generate and run migration successfully
- [ ] Typecheck passes

### US-002: Display priority indicator on task cards
**Description:** As a user, I want to see task priority at a glance so I know what needs attention first.

**Acceptance Criteria:**
- [ ] Each task card shows colored priority badge (red=high, yellow=medium, gray=low)
- [ ] Priority visible without hovering or clicking
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-003: Add priority selector to task edit
**Description:** As a user, I want to change a task's priority when editing it.

**Acceptance Criteria:**
- [ ] Priority dropdown in task edit modal
- [ ] Shows current priority as selected
- [ ] Saves immediately on selection change
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-004: Filter tasks by priority
**Description:** As a user, I want to filter the task list to see only high-priority items when I'm focused.

**Acceptance Criteria:**
- [ ] Filter dropdown with options: All | High | Medium | Low
- [ ] Filter persists in URL params
- [ ] Empty state message when no tasks match filter
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

## Functional Requirements

- FR-1: Add `priority` field to tasks table ('high' | 'medium' | 'low', default 'medium')
- FR-2: Display colored priority badge on each task card
- FR-3: Include priority selector in task edit modal
- FR-4: Add priority filter dropdown to task list header
- FR-5: Sort by priority within each status column (high to medium to low)

## Non-Goals

- No priority-based notifications or reminders
- No automatic priority assignment based on due date
- No priority inheritance for subtasks

## Technical Considerations

- Reuse existing badge component with color variants
- Filter state managed via URL search params
- Priority stored in database, not computed

## Success Metrics

- Users can change priority in under 2 clicks
- High-priority tasks immediately visible at top of lists
- No regression in task list performance

## Open Questions

- Should priority affect task ordering within a column?
- Should we add keyboard shortcuts for priority changes?
```

---

## Checklist

Before saving the PRD:

- [ ] Asked clarifying questions with lettered options
- [ ] Incorporated user's answers
- [ ] Captured assumptions explicitly when proceeding without answers
- [ ] User stories are small and specific
- [ ] User stories include dependencies where needed
- [ ] Functional requirements are numbered and unambiguous
- [ ] Non-goals section defines clear boundaries
- [ ] Security requirements were included when applicable
- [ ] API/data implications were included when applicable
- [ ] Delivery sequencing and parallelism were called out when needed
- [ ] Test expectations are sufficient for autonomous implementation
- [ ] Saved to `tasks/prd-[feature-name].md`
