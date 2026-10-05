I need a plan I can present to my Product Owners today. Do NOT change any code or
data. Only read the repo and the database (read-only SELECTs), then write a short
report in simple English. Don't mention file names, function names, or code; only
describe how things work and what we'd change. Use our own terms: use case, workflow,
project, PO, end user, AD group, entitlement.

## Situation
- 300 QA use cases (workflows) were promoted from UAT to prod.
- POs have been testing them in prod, so prod already contains PO test executions.
- POs approved the first batch of 50 to release to real end business users.
  The other 250 must stay hidden from end users for now.
- After release, POs may keep testing the same use cases, and end users will run
  them for real. These two kinds of data must not mix in end-user reports/exports.

## Our current access levels (RBAC)
- View: read-only access
- Edit: developer access, can modify configurations, tools, and agents
- Execute: can run the workflows (also includes Edit and View)
- Owner: full/administrative access

## Part 1: Explain how access works today
Investigate the repo and database and explain simply:
1. How AD groups are mapped to the 4 access levels above.
2. Whether access is granted per project, per use case, or globally.
3. Which AD groups / access levels POs have today.
4. How the app decides which projects, use cases, and execution data a user can see.
5. Whether anything today already marks a use case as "ready" or "released", or
   marks an execution as a test run vs a real run.

## Part 2: Evaluate this proposed plan against what you found
The idea:
- Keep every use case where it is (don't move or copy it into a new project).
- Add a release status to each use case: TESTING (POs only) → RELEASED (POs + end
  users) → PAUSED (no new runs, history kept).
- Tag every execution as TEST or LIVE automatically, based on who ran it:
  PO runs = TEST, end-user runs = LIVE. All existing prod executions become TEST.
- End users see only RELEASED use cases and only LIVE data in reports and Excel exports.
- A "Promote" action changes TESTING → RELEASED, requires a second person's approval,
  and every status change is recorded (who, when, which version, why).
- The approved version of the use case is locked; later edits need re-approval
  before end users get them.
- Bulk promote for a batch (e.g. the first 50), and Pause as an instant rollback.

For each point, tell me: does it fit our current design, what would need to change
(in plain words, e.g. "add a release status to use cases", "add a TEST/LIVE marker
to execution records"), and any risk or gap you see.

## Part 3: Answer the entitlement question
Recommend ONE of these, with pros and cons based on how our AD groups actually work:
  A) Create a new AD group / entitlement for end users (e.g. "end user: Execute +
     View, RELEASED use cases only, LIVE data only")
  B) Reuse the existing Execute / View levels and add the release-status rule on top
  C) Something better you find in our design
Also say whether end-user access should be per project/business unit or global.

## Part 4: Show the final access picture
A simple table: role (PO, developer, end user, owner) × what they can see/do:
- which use cases (TESTING / RELEASED)
- which data (TEST / LIVE)
- can promote? can approve? can pause?
Also recommend whether POs should see LIVE data too (to monitor real runs), or
TEST only.

## Part 5: Rollout and open questions
- The high-level steps to deliver this, in order (one-time database change via
  Liquibase CR, then app changes, then promoting batch 1).
- Confirm that future batches (the next 50, etc.) need NO new CR, only the Promote
  + approval flow.
- List the open questions the POs/compliance must decide before we build.

Keep the whole report to 1–2 pages, plain English, with tables where helpful.
