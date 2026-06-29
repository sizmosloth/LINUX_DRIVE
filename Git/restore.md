# git restore

**Purpose:**
*Used to discard changes made to files or restore files to a previous state.*

**Syntax:**

`git restore <file>`

**Examples:**

`git restore app.py`

Discard changes made to app.py.

`git restore README.md`

Restore README.md to its last committed version.

**Why Use It?**

Sometimes I edit a file and realize I don't want those changes anymore.

Instead of manually undoing everything, I can use:

`git restore`

**Real World Example:**

I accidentally changed Connector's backend code.

I want the file exactly how it was in the last commit.

I run:

`git restore server.js`

The file returns to its previous committed state.

**Things to Remember:**

- Works only on tracked files.
- Removes uncommitted changes.
- Be careful because the discarded changes cannot be recovered easily.

**Also :**

`git restore --staged`

To untrack a tracked file from git

**Practice:**

Create a file.

Change something inside it.

**Run:**

`git status`

Now restore it:

`git restore filename`

Run:

`git status`

**Challenge:**

Edit two files.

Restore only one.

Check which file still has changes.

**My Notes:**

*I use git restore when I want to discard changes made to a tracked file.*