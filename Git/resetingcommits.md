# git reset --hard

**Purpose:**

*Used to move back to a previous commit and make all files exactly like that commit.*

**Syntax:**

`git reset --hard <commit>`

**Examples:**

`git reset --hard HEAD`

Reset all files to the latest commit.

`git reset --hard HEAD~1`

Go back one commit.

`git reset --hard HEAD~2`

Go back two commits.

`git reset --hard <commit-hash>`

Go to a specific commit.

**What is HEAD?**

`HEAD`

Points to the current commit.

`HEAD~1`

Previous commit.

`HEAD~2`

Two commits back.

How It Works:

**Before:**

A <- B <- C (HEAD)

Run:

`git reset --hard HEAD~1`

**After:**

A <- B (HEAD)

Commit C is removed from the current branch.

Recovering a Commit:

If I reset by mistake, I can recover it using:

`git reflog`

Then:

`git reset --hard HEAD@{1}`

or

`git reset --hard <commit-hash>`

**Real World Example:**

I accidentally made a bad commit.

I used:

`git reset --hard HEAD~1`

Later I wanted that commit again.

I checked:

`git reflog`

Then restored it.

Things to Remember:

- Deletes all uncommitted changes.
- Changes the current commit.
- `git reflog` can usually recover lost commits.

**Practice:**

Create 3 commits.

Run:

`git reset --hard HEAD~1`

Recover it using:

`git reflog`

**My Notes:**

*I use `git reset --hard` when I want my project to look exactly like an older commit. If I reset by mistake, `git reflog` is my best friend.*