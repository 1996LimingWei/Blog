## Suppose you’re working on a branch search_function for a song search feature. Your commit history accidentally includes:

1. An unrelated commit f6cbe89 titled init join and create room (meant for another PR).
2. This commit was pushed to your search_function branch and is now visible in your PR.

# Step-by-Step Solution
1. Identify the Commit History
First, check your current commit history to locate the unwanted commit:

`git log`
### Output:
64e57a1 (HEAD -> search_function) update search func and button size

8eff407 add error handling

f6cbe89 init join and create room  # ❌ Unwanted commit

6512cc7 init search func

733680a Merge: 42b20bb f302277

2. Reset the Branch to Before the Unwanted Commit
Use git reset --hard to "rewind" your branch to the last valid commit (before the unwanted one). This removes the unwanted commit and all commits after it from the branch history. USE THIS CAREFULLY
```
# Replace <last-valid-commit-hash> with the commit BEFORE the unwanted one
git reset --hard 6512cc7
``

`git log` now shows:

6512cc7 (HEAD -> search_function) init search func

733680a Merge: 42b20bb f302277

The unwanted commit f6cbe89 is now removed.

3. Reapply Relevant Commits
If you had valid commits after the unwanted one (e.g., 8eff407 and 64e57a1), reapply them using git cherry-pick:
```
git cherry-pick 8eff407

git cherry-pick 64e57a1
```

If conflicts appear, resolve them in your code editor, then run git add . and git cherry-pick --continue.

4. Force-Push to Update the PR
Since you’ve rewritten the branch history, you must force-push to overwrite the remote branch:

`git push -f origin search_function`

5. Verify the PR
Check your PR on GitHub. The unwanted commit f6cbe89 should no longer appear, and only your relevant commits will be visible.
