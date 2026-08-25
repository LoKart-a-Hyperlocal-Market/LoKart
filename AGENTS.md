# LoKart — AI Development Rulebook & Guidelines

These rules apply to EVERY AI coding agent working on the LoKart repository. LoKart is a full-stack hyperlocal commerce platform. All AI agents must adhere strictly to these guidelines before inspecting, modifying, or committing code.

==================================================
1. GIT BRANCH RULES — CRITICAL
==================================================

### MAIN BRANCH

`main` is the stable/production branch.

AI MUST:
- NEVER modify code while on `main`.
- NEVER commit directly to `main`.
- NEVER push directly to `main`.
- NEVER force-push to `main`.
- NEVER delete, reset, or overwrite `main`.

Before doing ANY work, the AI must run:

```bash
git branch --show-current
git status
```

If the current branch is `main`, the AI MUST STOP IMMEDIATELY and tell the developer:
> "You are currently on the `main` branch. Please switch to `development` or create a feature branch before I make changes."

### DEVELOPMENT BRANCH & WORKFLOW

`development` is the primary development and integration branch, and it is the **final destination for all AI work**. There is no separate long-lived review branch unless the developer explicitly asks for one on a specific task.

**Pull vs. push are asymmetric.** The AI may *pull* (read) from both `main` and `development` — `main` is checked so any production hotfixes are known about and can be reconciled. The AI may only ever *push* (write) to `development`. `main` stays read-only for AI agents, always — see the pre-push rule in Section 8.

Standard workflow:
1. Start every session by syncing with both branches (fetch/pull, do not push to either yet):
   ```bash
   git fetch origin main development
   git checkout development
   git pull origin development
   git log --oneline development..origin/main   # check if main has commits not yet in development
   ```
   If `main` has commits that aren't in `development` (e.g. a production hotfix), note it and flag it to the developer before proceeding — do not silently merge `main` into `development` without confirmation unless the task explicitly asks for it.
2. For small/contained tasks, work directly on `development` locally (not yet pushed).
3. For larger or riskier tasks, isolate the work on a feature branch created from freshly-pulled `development`:
   ```bash
   git checkout -b feature/<feature-name>
   ```
   Naming convention: `feature/authentication`, `feature/product-system`, `feature/order-management`, `feature/retailer-dashboard`, `feature/delivery-logistics`.
4. Regardless of where work happened, integration into `development` always goes through the **Pre-Push Protocol** in Section 8 — never a raw `git push`.
5. NEVER push directly to `main`.

==================================================
2. PROJECT ARCHITECTURE & SEPARATION
==================================================

LoKart follows a clean, modular full-stack architecture.

### FRONTEND / BACKEND SEPARATION

Frontend code belongs inside:
```text
frontend/  (or src/ for single-repo Vite/TanStack Start architecture)
```

Backend code belongs inside:
```text
backend/
```

Database code belongs inside:
```text
database/
```

- **Frontend includes**: UI components, pages, layouts, styling/CSS, client hooks, client services, API contracts, frontend assets.
- **Backend includes**: API controllers, server routes, server-side authentication/authorization, database models, backend services, server utilities.
- **Database includes**: Schema definitions, migration scripts, seed files.

DO NOT put backend logic inside frontend code.
DO NOT put frontend UI logic inside backend code.

If a task requires full-stack changes:
1. Identify frontend scope.
2. Identify backend scope.
3. Implement each cleanly in its respective directory.
4. Connect frontend and backend through the designated API/service layer interface.

### DIRECTORY STRUCTURE CONVENTIONS

Adapt to the project framework (Vite + TanStack Start):

```text
LoKart/
│
├── src/
│   ├── routes/              # TanStack Router route definitions (__root, index, customer, delivery, retailer)
│   ├── features/            # Feature modules (auth, customer, delivery, retailer, landing)
│   ├── components/          # Shared base UI components (Radix UI / Shadcn)
│   ├── context/             # React Context Providers (AuthContext)
│   ├── services/            # Decoupled business service layer (AuthService)
│   ├── data/                # Static & Demo data definitions
│   ├── types/               # Shared TypeScript interfaces
│   ├── hooks/               # Custom React hooks (useExploreLoKart, useMotionEnv)
│   ├── lib/                 # Core utilities & error reporting
│   └── styles/              # Global CSS styles
│
├── backend/                 # Backend API services & controllers
├── database/                # Database migrations & schemas
├── docs/
│   └── ai-changes/          # Mandatory AI task change records
├── public/                  # Static assets
├── .env.example             # Environment template
└── README.md                # Documentation
```

Do NOT create arbitrary folders. Re-use existing structures where appropriate.

==================================================
3. EXISTING CODE HAS PRIORITY
==================================================

Before creating any file or component:
1. Search the repository.
2. Check whether the functionality already exists.
3. Re-use existing components, services, functions, utilities, and styles.
4. Follow existing project conventions.

Never create duplicate:
- Components
- Services
- Routes
- Utilities
- API clients
- Models
- Contexts
- Authentication systems

Do not replace working code unnecessarily.

==================================================
4. MINIMAL CHANGE PRINCIPLE
==================================================

AI MUST keep changes tightly focused on the requested task.

DO NOT:
- Redesign unrelated UI or layouts.
- Refactor unrelated files or modules.
- Rewrite the entire project for a small feature.
- Replace existing architecture unnecessarily.
- Change unrelated APIs or data structures.
- Modify unrelated configuration.
- Alter existing UI designs, layout, colors, spacing, or responsive behavior unless the task explicitly requires it.

If an out-of-scope issue is discovered:
> Document it in the **OUT-OF-SCOPE ISSUES** section of the final report. Do NOT silently modify or fix it.

==================================================
5. SECURITY & SECRETS
==================================================

NEVER commit, log, or expose secrets:
- API keys
- Passwords
- Access tokens
- Database credentials
- Private keys
- Authentication secrets
- Personal credentials

Strict Rules:
- Never hard-code secrets into source code, comments, documentation, `AGENTS.md`, or change logs.
- Use environment variables (`.env.local` for local secrets, `.env.example` for variable names and non-sensitive placeholders).
- Respect `.gitignore`. `.env.local` MUST NOT be committed.
- In `docs/ai-changes/`, list environment variable NAMES only. NEVER write actual secret values.

### AUTHENTICATION SECURITY
- Never store plaintext passwords.
- Relying solely on client-side role checks for authorization is strictly forbidden for production code; authorization must be verified server-side.
- Never expose private server secrets in client-facing environment variables (`VITE_` or `NEXT_PUBLIC_`).

### CUSTOMER DATA & PAYMENT INFORMATION
LoKart handles customer PII (names, addresses, phone numbers) and potentially payment data. AI MUST:
- Never log full customer PII or payment details to console, error trackers, or committed test fixtures.
- Never commit real customer data as seed/sample data — use clearly fake, synthetic values only.
- Treat any code path touching card numbers, CVVs, or bank details as PCI-DSS-sensitive: never store raw card data; rely on the existing payment provider/tokenization layer rather than building new handling logic.
- Flag any task that would require touching payment/PII handling directly in the final report, even if completed, so a human can double-check compliance implications.

==================================================
6. DEPENDENCY MANAGEMENT
==================================================

Before installing any new package:
1. Check `package.json`.
2. Check existing dependencies.
3. Determine whether an existing dependency already provides the needed capability.
4. Confirm the new dependency is strictly necessary.
5. Run a vulnerability check before adopting it (e.g. `npm audit` on the target package, or check its advisory history) and avoid packages with known unpatched critical/high vulnerabilities.

If a package is added, document in the change record:
- Package name & version
- Justification / purpose
- Where in the project it is consumed
- Result of the vulnerability check (PASS / issues found and accepted / NOT RUN)

==================================================
7. DESTRUCTIVE GIT & FILE OPERATIONS
==================================================

AI MUST NOT perform destructive operations without explicit user confirmation.

DO NOT automatically run:
- `git reset --hard`
- `git clean -fd`
- `git push --force` or `git push -f`
- `git branch -D`

### FILE DELETION SAFETY
Before deleting any file:
1. Search for all references and usages across the repository.
2. Check imports, routes, and configuration files.
3. Confirm the file is genuinely obsolete.
4. If uncertain, DO NOT delete the file; document it for developer review.

### DATABASE DESTRUCTIVE OPERATIONS
Schema/data changes carry higher risk than code changes because they can be irreversible in production. AI MUST NOT, without explicit developer confirmation:
- Drop tables, columns, or indexes.
- Write migrations that are not reversible (no corresponding `down`/rollback step).
- Run backfills or bulk updates/deletes against real data.
- Change column types in a way that can silently truncate or lose data.

All migrations should be written with a rollback path documented in the change record's **Database Changes** section, even if the rollback is "not applicable — additive only."

### GIT CONFLICT HANDLING
Never resolve merge conflicts by blindly choosing `ours`, `theirs`, `incoming`, or `current`.
1. Inspect both sides of the conflict.
2. Understand the intent of both changes.
3. Preserve both functional changes whenever possible.
4. If ownership or intent is ambiguous, STOP and ask the developer for guidance.

This rule applies every time conflicts appear — including during the mandatory pre-push pull described in Section 8, not just during manual merges.

==================================================
8. COMMITS & PUSHES
==================================================

### AUTOMATIC COMMIT RULE
Completed and verified AI implementation tasks SHOULD be committed automatically unless the developer explicitly instructs not to commit.

Every completed task must have a clear conventional commit message.

Examples:
- `feat(auth): add authentication flow`
- `feat(products): add product management`
- `fix(cart): prevent duplicate items`
- `refactor(api): separate service layer`
- `docs(ai): document authentication implementation`

NEVER use vague commit messages such as: `changes`, `update`, `stuff`, `final`, `fix`, `new`.

### PRE-PUSH PROTOCOL — MANDATORY, NO EXCEPTIONS

**An AI agent must never push without first pulling.** A "blind push" — pushing local commits without syncing against the latest remote state first — is prohibited, since other agents, developers, or a production hotfix may have moved things in the meantime.

**Pull from both `main` and `development`. Push only to `development`.** These are not symmetric: reading `main` keeps the AI aware of anything shipped straight to production (hotfixes) that `development` doesn't have yet; writing only ever targets `development`. `main` is never a push destination for an AI agent, under any circumstance — see Section 1.

Every push follows this exact sequence:

1. Commit all intended local changes first, so the working tree is clean.
2. Sync with the remote — fetch and pull **both** branches:
   ```bash
   git fetch origin main development
   git pull origin development
   git log --oneline development..origin/main   # anything on main not yet in development?
   ```
   If `main` has moved ahead with changes relevant to this task (e.g. a hotfix touching the same files), reconcile that into your work as part of conflict resolution below rather than ignoring it. If it's unrelated to the current task, note it in the change record's Known Issues instead of pulling it in unprompted.
3. **If the pull into `development` reports no conflicts** — proceed to step 6.
4. **If the pull reports conflicts** — resolve them using the Section 7 conflict rules: inspect both sides, understand intent, preserve both functional changes, and STOP and ask the developer if intent is ambiguous. Never auto-resolve with `--ours`/`--theirs`.
5. After resolving conflicts, **re-run build/lint/typecheck/tests** to confirm the merge didn't break anything. Do not skip this because tests passed before the merge.
6. Review `git diff` one final time and confirm zero secrets, no unintended files, and no PII/payment data introduced.
7. Push — **`development` only**:
   ```bash
   git push origin development
   ```
   (or the equivalent for the feature branch actually being integrated — but the final write destination is always `development`, never `main`.)
8. **If the push is rejected** (remote moved again since step 2), return to step 2 and repeat. Never use `--force` or `-f` to bypass this.

### FEATURE BRANCH INTEGRATION
If work was done on a `feature/<name>` branch, integrate it into `development` by running the same Pre-Push Protocol with `development` as the pull/push target — i.e., pull latest `development` into the feature branch, resolve conflicts, retest, then merge/push into `development`. Do not leave completed work stranded on a long-lived feature branch.

==================================================
9. MULTI-AGENT COORDINATION
==================================================

Multiple AI agents may work on this repository, sometimes concurrently. To avoid collisions:

- Always `git fetch origin main development` and `git pull origin development` at the **start** of a session, before planning work — not just before pushing. Pulling `main` is for awareness (hotfixes); it is never pushed to.
- Before starting a task, skim recent entries in `docs/ai-changes/` (most recent 3–5 files) to see if another agent has recently touched the same area, to avoid duplicate or conflicting work.
- If a long-running task is likely to take a while, prefer working on a feature branch (Section 1) so partial work doesn't block other agents from pushing to `development`.
- Change record numbering (Section 10) MUST be re-checked immediately before creating the file — not assumed from an earlier point in the session — since another agent may have added a record in the meantime.

==================================================
10. MANDATORY AI TASK DOCUMENTATION & HANDOFF
==================================================

EVERY completed AI task MUST create a change record to ensure total traceability across multiple AI agents and developers.

Record Location:
```text
docs/ai-changes/
```

File Naming Convention:
`001-task-name.md`, `002-task-name.md` (sequential 3-digit numbering).

**Collision avoidance:** Immediately before creating a new record, run `git pull origin development` and `ls docs/ai-changes/` to determine the true highest existing number — do not rely on a count taken earlier in the session. If, at push time, the number collides with one added by another agent in the interim, rename your file to the next free number before finalizing the commit.

### CHANGE RECORD TEMPLATE

```markdown
# [Task Name]

## Date
YYYY-MM-DD

## AI Agent
[Name/Version of AI coding agent]

## User Request
[Summarize the exact user request]

## Objective
[Explain what the task accomplished]

## Files Created
- `path/to/created-file.ext`

## Files Modified
- `path/to/modified-file.ext`

## Files Deleted
- `path/to/deleted-file.ext`

## Files Moved
- `old/path.ext` → `new/path.ext`

## Implementation Summary
[Detailed summary of changes implemented]

## Technical Changes
[Bullet points of key architectural/technical modifications]

## Dependencies Added
- `package-name@version`: [Reason] | [Vulnerability check: PASS / issues accepted / NOT RUN] (or "None")

## Environment Variables
- `VARIABLE_NAME`: [Description] (or "None")

## Database Changes
[Schema, migration, or model updates, including rollback path] (or "None")

## Merge Conflicts Encountered
[Description of any conflicts hit during the Pre-Push Protocol and how they were resolved] (or "None")

## Testing Checklist
- [ ] Build (PASS / FAIL / NOT RUN)
- [ ] Lint (PASS / FAIL / NOT RUN)
- [ ] Typecheck (PASS / FAIL / NOT RUN)
- [ ] Unit tests (PASS / FAIL / NOT RUN)
- [ ] Manual testing (PASS / FAIL / NOT RUN)
- [ ] Re-tested after conflict resolution, if applicable (PASS / FAIL / N/A)

## Git Information
- **Branch**: `branch-name`
- **Commit**: `hash / commit message`
- **Push Status**: `SUCCESS / PENDING / NOT DONE`

## Known Issues
- [Any unresolved issues or limitations]

## Future Work
- [Recommended follow-up tasks]
```

### PROMPT TRACEABILITY & HANDOFF
Every change record must allow any subsequent AI agent to trace:
`USER REQUEST` → `WHAT WAS IMPLEMENTED` → `FILES CHANGED` → `TESTS PERFORMED` → `CONFLICTS RESOLVED` → `COMMIT` → `PUSH` → `REMAINING WORK`.

==================================================
11. TESTING & VERIFICATION
==================================================

Before marking a task as complete:
1. Run existing project verification commands (e.g. `npm run build`, `npm run lint`).
2. Only run commands that actually exist in `package.json`. Do NOT invent test commands.
3. NEVER make false claims of success ("Everything works") without empirical verification.
4. Clearly distinguish execution status: `PASS`, `FAIL`, `NOT RUN`, `NOT VERIFIED`.
5. If conflicts were resolved during the Pre-Push Protocol (Section 8), re-run these checks after resolution — a pre-merge PASS does not carry over automatically.

==================================================
12. MANDATORY TASK CHECKLIST
==================================================

Before declaring ANY task complete, verify every item:

- [ ] Read `AGENTS.md` rules
- [ ] Executed `git branch --show-current` (Confirmed branch is NOT `main`)
- [ ] Executed `git status` and pulled both `main` and `development` at session start (push target remains `development` only)
- [ ] Inspected existing code & avoided duplicate implementations
- [ ] Identified Frontend / Backend / Database scope
- [ ] Kept changes minimal & focused on requested task
- [ ] Preserved existing UI designs, layout, colors, and responsive behavior
- [ ] Verified zero secrets or exposed PII/payment data in code, diff, or docs
- [ ] Checked new dependencies for known vulnerabilities (if any added)
- [ ] Ran build / lint / verification commands (when available)
- [ ] Followed the Pre-Push Protocol: pulled latest `development`, resolved any conflicts non-blindly, re-tested after resolution
- [ ] Reviewed `git diff` one final time before pushing
- [ ] Re-checked change record numbering immediately before creating the file
- [ ] Created change record in `docs/ai-changes/`
- [ ] Created conventional git commit
- [ ] Pushed to `development` (no force push, no bypass of Pre-Push Protocol)
- [ ] Provided complete final report to user

==================================================
13. FINAL REPORT FORMAT
==================================================

When completing a task, output a clear report:

```markdown
## Completed
[Summary of implemented work]

## Files Created
- list

## Files Modified
- list

## Files Deleted
- list

## Files Moved
- list

## Testing
- Build: PASS / FAIL / NOT RUN
- Lint: PASS / FAIL / NOT RUN
- Manual Testing: PASS / FAIL / NOT RUN

## Merge Conflicts Resolved
- [Description, or "None"]

## Git
- Branch: `<branch-name>`
- Commit: `<commit-hash>`
- Push: `SUCCESS / FAILED / NOT DONE`

## Documentation
- `docs/ai-changes/<record-name>.md`

## Out-of-Scope Issues Identified
- [Any unaddressed issues found during work]

## Known Issues & Next Steps
- [Remaining items or recommendations]
```

==================================================
14. FINAL GOLDEN RULE
==================================================

```text
CHECK (branch + status + pull main AND development)
  ↓
UNDERSTAND
  ↓
PLAN
  ↓
IMPLEMENT
  ↓
TEST
  ↓
DOCUMENT
  ↓
REVIEW DIFF
  ↓
COMMIT
  ↓
PULL main + development, RESOLVE CONFLICTS, RE-TEST
  ↓
PUSH — development ONLY, never main (retry on rejection, never force)
  ↓
REPORT
```

NEVER SKIP THESE STEPS. Never push without pulling first. Pull from both `main` and `development`; push only ever lands on `development`. Keep the LoKart repository safe, clean, maintainable, traceable, and ready for future development by developers and AI agents.