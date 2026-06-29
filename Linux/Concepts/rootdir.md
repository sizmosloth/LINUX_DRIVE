# Root Directory (/)

Purpose:
*The Root Directory is the starting point of the entire Linux filesystem.*

What is Root?

The Root Directory is represented by:

`/`

Every file and directory on Linux exists somewhere inside it.

Example:

```
/
|-- home
|-- bin
|-- etc
|-- usr
|-- var
|-- tmp
|-- root
|-- dev
```

Think Like This:

Imagine Linux is a tree.

The Root Directory (`/`) is the trunk.

Every folder grows from it.

Difference Between Root Directory and Root User

Root Directory:

`/`

The top-level directory.

Root User:

`root`

The administrator account with full permissions.

These are different things.

Real World Example:

My project:

`/home/sizmosloth/Documents/GitHub/LINUX_DRIVE`

starts from the Root Directory.

Why is it Important?

Linux always begins searching from `/`.

Every path eventually starts here.

Things to Remember:

- `/` is NOT the same as `/root`.
- `/` is the Root Directory.
- `/root` is the Home Directory of the root user.

Practice:

Run:

`cd /`

Run:

`pwd`

Run:

`ls`

Explore:

`cd /home`

`cd /usr`

`cd /etc`

My Notes:

*Everything in Linux starts from the Root Directory (`/`).*