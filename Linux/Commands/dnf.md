# dnf

Purpose:
*Used to install, update, remove and manage software packages on Fedora.*

Syntax:

`dnf <action> <package>`

Common Commands:

`sudo dnf install git`

Install Git.

`sudo dnf remove git`

Remove Git.

`sudo dnf update`

Update all installed packages.

`dnf search tor`

Search for a package.

`dnf info git`

Show information about a package.

Why Use It?

DNF downloads software from Fedora's official repositories and automatically installs any required dependencies.

Real World Example:

I wanted to install Git.

Instead of downloading it from a website, I simply used:

`sudo dnf install git`

Things to Remember:

- Fedora uses DNF as its package manager.
- Usually requires `sudo`.
- Automatically installs dependencies.
- Safer than downloading random files.

My Notes:

*DNF is Fedora's package manager. It's the first place I should check when installing software.*