# flatpak

Purpose:
*Used to install applications that work across many Linux distributions.*

Syntax:

`flatpak <action>`

Common Commands:

`flatpak search firefox`

Search for an application.

`flatpak install flathub org.mozilla.firefox`

Install Firefox.

`flatpak run org.mozilla.firefox`

Run the application.

`flatpak list`

Show installed Flatpak applications.

`flatpak update`

Update Flatpak applications.

`flatpak uninstall org.mozilla.firefox`

Remove an application.

Why Use It?

Flatpak allows developers to distribute the latest version of their applications without depending on the Linux distribution.

Real World Example:

I learned how to install Tor Browser using following command but i went for tar method:

`flatpak install flathub org.torproject.torbrowser-launcher`

Things to Remember:

- Works on many Linux distributions.
- Applications are sandboxed.
- Uses Application IDs.

My Notes:

*Flatpak is great for desktop applications that need the latest updates.*