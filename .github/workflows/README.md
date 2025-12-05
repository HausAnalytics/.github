# Organization Stale Management Workflow

Automatically manages stale issues and pull requests across all repositories in the HausAnalytics organization.

## What It Does

1. **Scans all non-archived repositories** in the organization
2. **Marks items as stale** after 30 days of inactivity by adding a `stale` label
3. **Removes stale label** if there's new activity on a stale item
4. **Closes stale items** after an additional 5 days of inactivity (if no activity)
5. **Sends notifications** (comments) only on PRs, not issues

## Schedule

- Runs daily at 1:30 AM UTC
- Scheduled runs are **dry run only** (no changes made)
- Manual runs can make actual changes

## Configuration

| Setting | Value |
|---------|-------|
| Days before stale | 30 |
| Days before close | 5 |
| Stale label | `stale` |
| Exempt PR label | `do not close` |

## How to Use

### Run Manually

1. Go to **Actions** tab in the `.github` repository
2. Select **Organization Stale Issues and PRs Management**
3. Click **Run workflow**
4. Options:
   - **dry_run**: Set to `true` to preview changes without making them
   - **org_name**: Organization to scan (defaults to HausAnalytics)

### Exempt a PR from Stale Management

Add the `do not close` label to any PR you want to keep open indefinitely.

**To add the label:**
1. Open the PR
2. Click **Labels** in the right sidebar
3. Type `do not close`
4. If the label doesn't exist, click **Create new label "do not close"**
5. Select the label

PRs with this label will be skipped entirely - they won't be marked stale or closed.

## Stale Label Removal

If a PR or issue has the `stale` label but receives new activity (comment, commit, etc.), the workflow automatically removes the stale label on the next run. This resets the close countdown.

**Example timeline:**
1. Day 0: PR opened
2. Day 30: No activity, PR marked as stale
3. Day 32: Author pushes a commit
4. Day 33 (next workflow run): Stale label removed
5. Day 63: If no further activity, PR marked as stale again

## Notifications

| Item Type | Marked Stale | Closed |
|-----------|--------------|--------|
| Issues | No notification | No notification |
| PRs | Comment posted | Comment posted |

PR authors receive GitHub notifications when their PR is marked stale or closed.

## Label Management

The workflow automatically creates required labels in each repository if they don't exist:

| Label | Color | Description |
|-------|-------|-------------|
| `stale` | Gray | Marks inactive issues/PRs |
| `do not close` | Green | Exempts PRs from stale management |

No manual label creation is needed.

## Summary Report

After each run, a summary is generated showing:
- Total items fetched, marked stale, and closed
- Current count of stale items across the organization
- Items about to be closed on the next run
- Job status

## Files

| File | Purpose |
|------|---------|
| `org-stale-management.yaml` | Main workflow file |

