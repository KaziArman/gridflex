# GridFlex Git Notes

This file summarizes the Git and GitHub instructions used for this project.

## Check That You Are in the Right Folder

Before running Git commands, make sure your terminal is inside the project folder:

```powershell
cd C:\gridflex
git status
```

If you see this error:

```text
fatal: not a git repository (or any of the parent directories): .git
```

then the current folder is not a Git repository. Either move to the correct folder or initialize Git:

```powershell
git init
```

## Create a New GitHub Repo and Push

`git push` does not automatically create a GitHub repository. Create the repo on GitHub first, then connect your local folder to it.

Basic first-time setup:

```powershell
cd C:\gridflex
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/KaziArman/gridflex.git
git branch -M main
git push -u origin main
```

Git tracks files, not empty folders. If you need to keep an empty folder, add a placeholder file such as `.gitkeep`.

## Make Sure You Push to the Correct Repo

Check which GitHub repository your local project is connected to:

```powershell
git remote -v
```

For this project, it should show:

```text
origin  https://github.com/KaziArman/gridflex.git (fetch)
origin  https://github.com/KaziArman/gridflex.git (push)
```

If it shows the wrong repository, change it:

```powershell
git remote set-url origin https://github.com/KaziArman/gridflex.git
```

Then verify again:

```powershell
git remote -v
```

Push to the correct repo:

```powershell
git push -u origin main
```

If your branch is not called `main`, check it:

```powershell
git branch --show-current
```

You can either push that branch:

```powershell
git push -u origin your-branch-name
```

or rename it to `main`:

```powershell
git branch -M main
git push -u origin main
```

## Push a New Folder

To push a new folder after your repo is already connected:

```powershell
git status
git add path/to/new-folder
git commit -m "Add new folder"
git push
```

To add everything changed in the project:

```powershell
git add .
git commit -m "Update project files"
git push
```

## If GitHub Rejects the Push with "fetch first"

This means the GitHub repo has commits that your local repo does not have.

Pull the remote changes first:

```powershell
git pull origin main --allow-unrelated-histories
```

Then push again:

```powershell
git push -u origin main
```

Only use force push if you intentionally want to overwrite the GitHub history:

```powershell
git push -u origin main --force
```

Be careful: force push can replace commits that are already on GitHub.

## Fix Merge Conflicts

If Git says there are conflicts, check the status:

```powershell
git status
```

Open each conflicted file and look for markers like:

```text
<<<<<<< HEAD
your local version
=======
GitHub version
>>>>>>> origin/main
```

Edit the file so only the correct final content remains. Then commit the merge:

```powershell
git add .
git commit -m "Merge remote changes"
git push -u origin main
```

For a quick conflict resolution where you keep your local version of specific files:

```powershell
git checkout --ours path/to/file
git add .
git commit -m "Merge remote changes keeping local files"
git push -u origin main
```

For keeping the GitHub version instead:

```powershell
git checkout --theirs path/to/file
git add .
git commit -m "Merge remote changes keeping GitHub files"
git push -u origin main
```

## Go Back to a Previous Commit from GitHub UI

Safest option: revert a commit.

1. Open the GitHub repo.
2. Click **Commits**.
3. Open the commit you want to undo.
4. Click **Revert**, if GitHub shows the button.
5. Commit directly or open a pull request.

This creates a new commit that reverses the old one.

For a single file:

1. Open the file on GitHub.
2. Click **History**.
3. Open the older version.
4. Click **View file**.
5. Copy or restore the old contents.
6. Commit the change.

GitHub UI usually does not reset a whole branch backward like `git reset --hard`. That is normally done from the terminal and may require a force push.

## Before Pushing

Always check what will be committed:

```powershell
git status
```

Make sure you are not committing secrets, private keys, `.env` files, credentials, or generated files that should stay local.
