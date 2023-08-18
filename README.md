# How use it

```
Usage: gh cutoff -o <owner> -r <repo> -b <branch>

Options:
  -o owner        Specify the owner of the repository you want to cutoff a branch
  -r repo         Specify the repository you want to cutoff a branch
  -b branch       Specify the branch you want to cutoff
```

# How it works

1. Update Protected Rule of the chosen branch to allow force push with the following rule
```json
{
    "required_status_checks": {
      "strict": true,
      "contexts": []
    },
    "enforce_admins": true,
    "required_pull_request_reviews": null,
    "restrictions": {
      "users": [],
      "teams": [],
      "apps": []
    },
    "required_linear_history": false,
    "allow_force_pushes": true,
    "allow_deletions": false,
    "block_creations": false,
    "required_conversation_resolution": true,
    "lock_branch": false,
    "allow_fork_syncing": false
}
```
Detail: not require a PR for pushing code and allow force push

2. Checkout the chosen branch (which we want to cut off)
3. Pull code from origin to make sure it's updated to date
4. Checkout new branch (the backup branch) from the chosen branch then push it to origin 
5. Checkout master
6. Delete the chosen branch
7. Checkout the chosen branch from master
8. Force push the chosen branch to origin
9. Update Protected Rule of the chosen branch to protected version with the follow rule
```json
{
    "required_status_checks": {
      "strict": true,
      "contexts": []
    },
    "enforce_admins": true,
    "required_pull_request_reviews": {
      "dismissal_restrictions": {},
      "dismiss_stale_reviews": true,
      "require_code_owner_reviews": false,
      "required_approving_review_count": 1,
      "require_last_push_approval": false,
      "bypass_pull_request_allowances": {}
    },
    "restrictions": {
      "users": [],
      "teams": [],
      "apps": []
    },
    "required_linear_history": false,
    "allow_force_pushes": false,
    "allow_deletions": false,
    "block_creations": false,
    "required_conversation_resolution": true,
    "lock_branch": false,
    "allow_fork_syncing": false
}
```