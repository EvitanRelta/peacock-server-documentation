# Generating SSH key-pair

This guide is on generating a SSH key-pair to be used for authentication when SSH-ing into a remote server/cluster.

<br>

## For Windows 10 _(build 1809 or later)_ and Windows 11

> ⚠️ **For Windows 7/8/8.1 or Windows 10 _(earlier than build 1809)_:** \
> OpenSSH _(which is needed for `ssh-keygen`)_ is not preinstalled. Use [PuTTY](https://www.putty.org) or install OpenSSH manually.

As OpenSSH is preinstalled on Windows 10 _(build 1809 or later)_ and Windows 11, `ssh-keygen` should be available in any terminal on your PC.

Run `ssh-keygen` in any terminal _(eg. CMD, PowerShell, Git-Bash)_ to create an Ed25519 key-pair.

It will prompt for:

-   Path to save the keys _(leave empty for default path)_
-   Passphrase to encrypt the keys with _(**DO NOT** leave empty, for security reasons)_

**Example usage:**

```
C:\Users\myuser> ssh-keygen

Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\myuser/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\myuser/.ssh/id_ed25519
Your public key has been saved in C:\Users\myuser/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:UQiyMbYdKm9S9WaXxxXkms2mCyQZFcUmB7nrA+l/qv8 myuser@LAPTOP-V66PNAGT
The key's randomart image is:
+--[ED25519 256]--+
|    = +. .=*..+. |
|   . X o.ooo+o   |
|  . = . * o=o .  |
|   +   o =.. =   |
|  . o   S...o +  |
|   o    oo.  o   |
|       . o. .    |
|        . o...   |
|        .++=E    |
+----[SHA256]-----+
```

By default, this will generate a public-private key-pair at these locations:

-   `C:\Users\[USERNAME]\.ssh\id_ed25519` - private key
-   `C:\Users\[USERNAME]\.ssh\id_ed25519.pub` - public key

> ⚠️ **If the `id_ed25519` save file exists already:** \
> **NO NEED** to overwrite the file. You can use the existing key-pair files to connect to your server. A single key-pair can be used for multiple different remote servers without problem.
