# whereis

Purpose:
*Used to locate the binary, source, and manual page of a command.*

Syntax:

`whereis <command>`

Examples:

`whereis python`

`whereis git`

`whereis bash`

Output:

`git: /usr/bin/git /usr/share/man/man1/git.1.gz`

This shows:

- Binary (Executable)
- Manual Page
- Other related files

Difference Between `which` and `whereis`

`which`

Shows only the executable being used.

Example:

`which python`

Output:

`/usr/bin/python`

`whereis`

Shows executable, manuals and other related files.

Example:

`whereis python`

Output:

`python: /usr/bin/python /usr/share/man/man1/python.1.gz`

Real World Example:

I want to know where Git is installed and where its documentation exists.

I use:

`whereis git`

Things to Remember:

- Shows more information than `which`.
- Very useful for exploring Linux.

Practice:

Run:

`whereis python`

Run:

`whereis bash`

Run:

`whereis ssh`

Challenge:

Compare the output of:

`which git`

and

`whereis git`

My Notes:

*I use `whereis` when I want more information than `which` can provide.*