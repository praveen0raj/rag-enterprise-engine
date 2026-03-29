---
description: Create technical documents (tech design, test design) for a PBI with GitHub Issue/Discussion and PR. Requires acceptance_criteria.md to already exist. Invoke with optional PBI name argument.
---

## Phase 0: Input & Setup

**Goal**: Identify the PBI, verify prerequisites, create GitHub artifacts, and prepare the branch.

### Step 0.0: Pull Latest Specs

Ensure the specs submodule is up to date before doing anything:

```bash
cd {PROJECT_ROOT}/specs && git checkout main && git pull origin main
```

If this fails (e.g., uncommitted changes in specs/), ask the user to resolve before continuing.

### Step 0.1: Identify the PBI

**If argument provided** (e.g., `/pbi-tech-doc e001-003_budget-edit`):
- Search for matching directory: `find {PROJECT_ROOT}/specs/epics -type d -name "*{argument}*" -path "*/pbis/*"`
- If exactly one match → use it
- If multiple matches → present list, let user pick
- If no match → STOP: "PBI directory not found in specs/"

**If no argument**:
- Scan for all PBIs: `find {PROJECT_ROOT}/specs/epics -type d -mindepth 3 -maxdepth 3 -path "*/pbis/*"`
- Present a numbered list via AskUserQuestion
- If no PBIs found → STOP: "No PBIs found. Run epic-kickoff to create the EPIC and PBIs first."

Store: `EPIC_NAME`, `PBI_NAME`, `PBI_DIR` (absolute path)

### Step 0.2: Verify PBI Issue Exists

Search for the PBI Issue on `{GITHUB_REPO}`:

```bash
gh issue list --repo {GITHUB_OWNER}/{GITHUB_REPO} --search "{PBI_NAME}" --json number,title,url --limit 10
```

- **Found** → store `PBI_ISSUE_NUM` and `PBI_ISSUE_URL`, continue
- **Not found** → STOP: "No PBI Issue found on {GITHUB_REPO}. Run epic-kickoff to create PBI Issues first."

### Step 0.3: Verify Acceptance Criteria Exists (GATE)

Check for `acceptance_criteria.md` in the PBI directory (in the PBI directory):

```bash
find {PBI_DIR} -name "acceptance_criteria.md" -type f
```

- **Found** → Read it, display a brief summary to the user, continue
- **Not found** → **STOP**: "No acceptance_criteria.md found. Create AC first, then re-run /pbi-tech-doc."

This gate is non-negotiable. Do NOT offer to create AC. Do NOT proceed without it.

### Step 0.4: Focus Directories

`FOCUS_DIRS` = entire repo **except `specs/`**. The `specs/` submodule is where we write output, not source code to read.

No question needed — this is automatic.

### Step 0.5: Create Branch in specs/ Submodule

```bash
cd {PROJECT_ROOT}/specs && git checkout -b tech-doc/{PBI_NAME}
```

### Step 0.6: Get GitHub User & Labels

```bash
GH_USER=$(gh api user --jq '.login')
```

Ask user to confirm assignee. Fetch `area/*` labels:

```bash
gh api repos/{GITHUB_OWNER}/{GITHUB_REPO}/labels --jq '.[].name' | grep '^area/'
```

AskUserQuestion to select applicable area labels.

**Label check**: Before using `tech-doc` label, verify it exists:
```bash
gh api repos/{GITHUB_OWNER}/{GITHUB_REPO}/labels --jq '.[].name' | grep '^tech-doc$'
```
If not found, create it: `gh label create tech-doc --repo {GITHUB_OWNER}/{GITHUB_REPO} --color "0075ca"`

### Step 0.7: Create Tech Doc Issue

```bash
gh issue create \
  --repo {GITHUB_OWNER}/{GITHUB_REPO} \
  --title "Tech Doc: {PBI display name}" \
  --label "tech-doc,{selected area/* labels}" \
  --assignee "{GH_USER}" \
  --body "## Tech Doc: {PBI display name}

**Parent PBI:** #{PBI_ISSUE_NUM}
**Discussion:** (updated after creation)
**Branch:** \`tech-doc/{PBI_NAME}\`

### Documents
- [x] acceptance_criteria.md (pre-existing)
- [ ] tech_design.md
- [ ] test.md
"
```

Store `TECH_ISSUE_NUM` and `TECH_ISSUE_URL`.

Set Issue type to Task and link as sub-issue of PBI Issue:

```bash
ISSUE_ID=$(gh api graphql -F query='{ repository(owner:"{GITHUB_OWNER}", name:"{GITHUB_REPO}") { issue(number: TECH_ISSUE_NUM) { id } } }' --jq '.data.repository.issue.id')
TASK_TYPE_ID=$(gh api graphql -F query='{ repository(owner:"{GITHUB_OWNER}", name:"{GITHUB_REPO}") { issueTypes(first:10) { nodes { id name } } } }' --jq '.data.repository.issueTypes.nodes[] | select(.name=="Task") | .id')
gh api graphql -F query='mutation { updateIssueIssueType(input: { issueId: "ISSUE_ID_VALUE", issueTypeId: "TASK_TYPE_ID_VALUE" }) { issue { id } } }'

PARENT_ID=$(gh api graphql -F query='{ repository(owner:"{GITHUB_OWNER}", name:"{GITHUB_REPO}") { issue(number: PBI_ISSUE_NUM) { id } } }' --jq '.data.repository.issue.id')
gh api graphql -F query='mutation { addSubIssue(input: { issueId: "PARENT_ID_VALUE", subIssueId: "ISSUE_ID_VALUE" }) { issue { id } subIssue { id } } }'
```

### Step 0.8: Create Discussion

```bash
REPO_ID=$(gh api graphql -F query='{ repository(owner:"{GITHUB_OWNER}", name:"{GITHUB_REPO}") { id } }' --jq '.data.repository.id')
CAT_ID=$(gh api graphql -F query='{ repository(owner:"{GITHUB_OWNER}", name:"{GITHUB_REPO}") { discussionCategories(first:10) { nodes { id name } } } }' --jq '.data.repository.discussionCategories.nodes[] | select(.name=="General") | .id')

gh api graphql \
  -F query='mutation($repoId: ID!, $catId: ID!, $title: String!, $body: String!) { createDiscussion(input: { repositoryId: $repoId, categoryId: $catId, title: $title, body: $body }) { discussion { number url id } } }' \
  -F repoId="$REPO_ID" \
  -F catId="$CAT_ID" \
  -F title="Tech Doc: {PBI display name}" \
  -F body="## Tech Doc: {PBI display name}

Discussion thread for technical documentation.

**PBI Issue:** {PBI_ISSUE_URL}
**Tech Doc Issue:** {TECH_ISSUE_URL}
**Branch:** \`tech-doc/{PBI_NAME}\`"
```

Store `DISC_NUM`, `DISC_URL`, `DISC_NODE_ID`. Then update Tech Doc Issue body with Discussion link.

### Step 0.9: Write State File

Create `.pbi-tech-doc-state.md` with all collected values. Set `Current phase: 1`, `Last completed step: 0.9`.

---

## Phase 1: Document Pipeline (Delegates to PBI Skills)

**Goal**: Create tech design and test design by delegating to dedicated PBI skills with correct specs/epics/ paths.

**Before starting**: Read `.pbi-tech-doc-state.md` if context seems incomplete.

### Step 1.0: Check Existing Tech Documents

Check which documents exist in `{PBI_DIR}/`:
- `tech_design.md`
- `test.md`

For each that exists and has real content (not just a "TODO" placeholder), ask:
- "{document} already exists. Skip or recreate?"

### Step 1.1: Tech Design

**Invoke `create-tech-design`** (Skill tool: `skill: "create-tech-design"`, `args: "{PBI_DIR}"`).

This skill handles:
- Two-stage design (policy → detailed)
- Design context check (Figma/.drawio files)
- Tech notes migration from AC
- Q&A cycles with question format
- Decision record option

- Code investigation scoped to parent repo

Wait for completion.

**Intermediate commit** after tech design is complete:
```bash
cd {PROJECT_ROOT}/specs && git add -A && git commit -m "Add tech design for {PBI_NAME}"
```

Update `.pbi-tech-doc-state.md`: `Last completed step: 1.1`.

### Step 1.2: Review Tech Design (Optional)

AskUserQuestion: "Run AI review on tech design?"
- **Yes (Recommended)** → Invoke `review-tech-design` (Skill tool: `skill: "review-tech-design"`, `args: "{PBI_DIR}"`)
- **Skip review** → proceed to Step 1.3

### Step 1.3: Test Design

**Invoke `create-test-design`** (Skill tool: `skill: "create-test-design"`, `args: "{PBI_DIR}"`).

This skill handles:
- 6-step analysis (modifications, impact, risks, test cases, Q&A, AC/TD updates)
- Automated vs manual test classification
- Existing test detection
- Regression test identification

- Code investigation scoped to parent repo

Wait for completion.

**Intermediate commit** after test design is complete:
```bash
cd {PROJECT_ROOT}/specs && git add -A && git commit -m "Add test design for {PBI_NAME}"
```

Update `.pbi-tech-doc-state.md`: `Last completed step: 1.3`.

### Step 1.4: Review Test Design (Optional)

AskUserQuestion: "Run AI review on test design?"
- **Yes (Recommended)** → Invoke `review-test-design` (Skill tool: `skill: "review-test-design"`, `args: "{PBI_DIR}"`)
- **Skip review** → proceed to Step 1.5

### Step 1.5: Update Issue

Update Tech Doc Issue to check off completed documents:

```bash
gh issue edit {TECH_ISSUE_NUM} --repo {GITHUB_OWNER}/{GITHUB_REPO} --body "## Tech Doc: {PBI display name}

**Parent PBI:** #{PBI_ISSUE_NUM}
**Discussion:** {DISC_URL}
**Branch:** \`tech-doc/{PBI_NAME}\`

### Documents
- [x] acceptance_criteria.md (pre-existing)
- [x] tech_design.md
- [x] test.md
"
```

Update `.pbi-tech-doc-state.md`: `Current phase: 2`, `Last completed step: 1.5`.

---

## Phase 2: Review & PR

**Goal**: Push and create PR.

### Step 2.1: Push

```bash
cd {PROJECT_ROOT}/specs && git push -u origin tech-doc/{PBI_NAME}
```

### Step 2.2: Create PR

```bash
gh pr create \
  --repo {GITHUB_OWNER}/{GITHUB_REPO} \
  --title "Tech Doc: {PBI display name}" \
  --label "tech-doc,{selected area/* labels}" \
  --assignee "{GH_USER}" \
  --body "$(cat <<'EOF'
## Tech Doc: {PBI display name}

### Summary
Technical documentation for PBI: {PBI display name}

### Documents
- `tech_design.md` — technical design (policy + detailed)
- `test.md` — test design

### Links
- **PBI Issue:** #{PBI_ISSUE_NUM}
- **Tech Doc Issue:** #{TECH_ISSUE_NUM}
- **Discussion:** {DISC_URL}

Closes #{TECH_ISSUE_NUM}
EOF
)"
```

Store `PR_NUM` and `PR_URL`.

### Step 2.3: Update Issue & Discussion

Update Tech Doc Issue with PR link. Comment on Discussion with PR link:

```bash
gh api graphql \
  -F query='mutation AddComment($id: ID!, $body: String!) { addDiscussionComment(input: { discussionId: $id, body: $body }) { comment { id } } }' \
  -F id="{DISC_NODE_ID}" \
  -F body="PR created: {PR_URL}"
```

### Step 2.4: Clean Up

Delete `.pbi-tech-doc-state.md` after presenting the final summary to the user.

---

## Error Handling

| Scenario | Action |
|----------|--------|
| PBI directory not found | STOP: "PBI directory not found in specs/. Check the path." |
| PBI Issue not found | STOP: "No PBI Issue found on {GITHUB_REPO}. Run epic-kickoff first." |
| acceptance_criteria.md not found | STOP: "No acceptance_criteria.md in PBI directory. Create AC first, then re-run /pbi-tech-doc." |
| Label doesn't exist | Create it automatically |
| GraphQL escaping error | Use `-F` flag, retry with explicit variable substitution |
| Push fails | Check if branch exists remotely. Ask user to verify SSH keys/permissions |
| PR creation fails | Show error, ask user to resolve and retry |
| Context compaction | Read `.pbi-tech-doc-state.md` to recover state |

## Context Recovery After Compaction

If at any point during the workflow you are unsure what phase or step you are in:

1. Read `.pbi-tech-doc-state.md`
2. `cd {PROJECT_ROOT}/specs && git branch --show-current` to confirm the branch
3. `cd {PROJECT_ROOT}/specs && git log --oneline -5` to see what's been committed
4. Resume from the next incomplete step as indicated by `Last completed step`

**Never guess workflow state. Always read the state file.**