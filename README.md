📌 Difference Between git fetch and git pull
🔹 git fetch

Downloads latest changes from remote repository.

Updates remote tracking branch (origin/main).

Does NOT change your current working branch.

Safe operation.

No automatic merge happens.

Example:

git fetch origin
🔹 git pull

Downloads latest changes from remote repository.

Automatically merges changes into your current branch.

Combination of:

git fetch + git merge

Example:

git pull origin main
✅ Main Difference
git fetch	git pull
Only downloads changes	Downloads + merges
No changes in working directory	Updates working directory
Safe	May cause conflict
📌 How to Resolve a Git Merge Conflict (Example Scenario)

When two branches modify the same line in a file, Git cannot decide which change to keep.

Git shows:

<<<<<<< HEAD
Your changes
=======
Incoming changes
>>>>>>> branch-name
Steps to resolve:

Open the file.

Remove conflict markers.

Keep the correct code.

Save the file.

Add file to staging:

git add filename

Commit:

git commit -m "Conflict resolved"

Conflict resolved successfully.

Save file and push:

git add README.md
git commit -m "Added README explanation"
git push origin main
