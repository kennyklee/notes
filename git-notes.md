# New Branch (Github)
git checkout -b [branchname]
git push origin [branchname]

# Clone Remote Branch
git remote show origin
git checkout -b [localRepoName] [origin/repo]

# Delete local
git branch -d [local]

# Delete Remote
git push origin --delete [branchname]

# Set Remote URL
git remote set-url origin [https://...]

# Reset to Remote Repo
// Save work first
git commit -am "Saving work"
git branch my-saved-work

// Reset
git fetch origin
git reset --hard origin/master

# Undo
git reset --hard <sha1>
