# stalebranch-cleanup-gitaction
GitHub Actions workflow for stale branch cleanup with multi-layered safety


## Cleanup Stale Branches

This repository includes a GitHub Actions workflow for safely cleaning up stale branches with multiple safety features.

### Safety Features

The cleanup workflow includes the following safety mechanisms:

1. **Protected Branch Exclusion**: Automatically skips branches marked as protected in GitHub
2. **Pattern-Based Exclusion**: Configurable patterns to exclude important branches (e.g., `main`, `master`, `develop`, `release/*`, `hotfix/*`)
3. **Active PR Check**: Branches with open pull requests are never deleted
4. **Deletion Limits**: Configurable maximum number of branches to delete per run
5. **User Confirmation**: Requires explicit confirmation by typing "DELETE" before any deletions occur
6. **Detailed Logging**: Comprehensive console output for complete transparency
7. **Dry Run Mode**: Test the cleanup process without actually deleting branches

### How to Use

The cleanup workflow is **manual only** and must be triggered explicitly:

1. Go to the **Actions** tab in the GitHub repository
2. Select the **Cleanup Stale Branches** workflow
3. Click **Run workflow**
4. Configure the workflow parameters:
   - **Confirm deletion**: Type `DELETE` to confirm (required)
   - **Days stale**: Number of days a branch must be inactive (default: 90)
   - **Max deletions**: Maximum branches to delete in this run (default: 10)
   - **Exclude patterns**: Comma-separated patterns to exclude (default: `main,master,develop,release/*,hotfix/*`)
   - **Dry run**: Enable to see what would be deleted without actually deleting (default: true)
5. Click **Run workflow**

### Example Usage

**Dry Run (Recommended First Step)**:
- Confirm deletion: `DELETE`
- Days stale: `90`
- Max deletions: `10`
- Exclude patterns: `main,master,develop,release/*,hotfix/*`
- Dry run: `true` ✅

This will show you which branches would be deleted without actually deleting them.

**Actual Cleanup**:
- Confirm deletion: `DELETE`
- Days stale: `90`
- Max deletions: `10`
- Exclude patterns: `main,master,develop,release/*,hotfix/*`
- Dry run: `false` ❌

This will actually delete the stale branches.

### What Gets Deleted

A branch is considered stale and eligible for deletion if:
- Its last commit is older than the specified number of days
- It is NOT a protected branch
- It does NOT match any exclude patterns
- It does NOT have any open pull requests
- The deletion limit has not been reached

### Best Practices

1. **Always run in dry run mode first** to review what would be deleted
2. **Start with conservative settings** (e.g., 90+ days, low max deletions)
3. **Review the exclude patterns** to ensure important branches are protected
4. **Check the workflow logs** to see detailed information about skipped and deleted branches
5. **Adjust deletion limits** based on your team's workflow and comfort level
