## Cleanup Stale Branches

This repository includes a GitHub Actions workflow for safely cleaning up stale branches with multiple safety features, including mandatory approval before deletion.

### Safety Features

The cleanup workflow includes the following safety mechanisms:

1. **Protected Branch Exclusion**: Automatically skips branches marked as protected in GitHub
2. **Pattern-Based Exclusion**: Configurable patterns to exclude important branches (e.g., `main`, `master`, `develop`, `release/*`, `hotfix/*`)
3. **Active PR Check**: Branches with open pull requests are never deleted
4. **Deletion Limits**: Configurable maximum number of branches to delete per run
5. **User Confirmation**: Requires explicit confirmation by typing "DELETE" before any deletions occur
6. **Detailed Logging**: Comprehensive console output for complete transparency
7. **Mandatory Approval**: Requires approval from a designated approver or repo admin before deleting branches
8. **Two-Stage Process**: Identification and deletion are separated into two jobs, allowing review before deletion

### Prerequisites for Approval

Before using the cleanup workflow, you must configure the GitHub environment for approvals:

1. Go to your GitHub repository **Settings**
2. Navigate to **Environments**
3. Create a new environment named `stale-branch-deletion-approval` (exact name required)
4. Under **Deployment protection rules**, enable **Required reviewers**
5. Add the approver(s):
   - If you specified an approver in the workflow input, add that GitHub user
   - If no approver is specified in the workflow input, add repository admins
6. **Important**: At least one approval is mandatory before branches can be deleted
7. Click **Save protection rules**

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
   - **Approver**: GitHub handle or email of approver (leave empty to use repo admin as default)
5. Click **Run workflow**
6. **Wait for the identification job to complete** - this will analyze and list all stale branches
7. **Review the identified branches** in the workflow logs
8. **Approval step**: If branches are found, the workflow will pause and wait for approval
   - The designated approver (or repo admin if none specified) will receive a notification
   - The approver must review and approve the deletion before it proceeds
   - Only after approval is granted will the deletion job execute
9. The deletion job will execute after approval is granted and delete the identified stale branches

### Example Usage

**Branch Cleanup**:
- Confirm deletion: `DELETE`
- Days stale: `90`
- Max deletions: `10`
- Exclude patterns: `main,master,develop,release/*,hotfix/*`
- Approver: `octocat` (or leave empty for repo admin)

This will:
1. Identify stale branches (those inactive for 90+ days)
2. Display the list in the workflow logs
3. Wait for approval from the designated approver
4. Delete the approved branches after approval is granted

**Note**: The two-stage process (identify then delete with approval in between) provides an opportunity to review the identified branches before they are deleted. This eliminates the need for a separate dry run mode.

### What Gets Deleted

A branch is considered stale and eligible for deletion if:
- Its last commit is older than the specified number of days
- It is NOT a protected branch
- It does NOT match any exclude patterns
- It does NOT have any open pull requests
- The deletion limit has not been reached
- **Approval has been granted by the designated approver or repo admin**

### Best Practices

1. **Review the workflow logs** after the identification job completes to see which branches were identified
2. **Start with conservative settings** (e.g., 90+ days, low max deletions)
3. **Review the exclude patterns** to ensure important branches are protected
4. **Check the identification summary** before granting approval
5. **Adjust deletion limits** based on your team's workflow and comfort level
6. **Configure the environment with appropriate approvers** before running the workflow
7. **Carefully review the list of identified branches** in the logs before granting approval for deletion
