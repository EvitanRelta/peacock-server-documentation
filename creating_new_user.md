# Creating new user on peacock server

This guide is on creating a new account on the peacock server, giving a new user access to the account.

<br>

## Create new user account _(as the admin)_

You'll need to be logged in as an existing user with admin permissions.

Use the `adduser` command to create a user interactively:

```bash
sudo adduser [NEW_USER]
```

-   It will prompt for a password
-   Then, it asks for user details, which you can leave blank

The new user will then be able to access the peacock server at _(after setting up SSH key and while in SoC wifi/VPN)_:

```bash
ssh [NEW_USER]@peacock.d2.comp.nus.edu.sg
```

<br>

## Generating SSH key-pair _(as the new user)_

The peacock server has password login disabled, so SSH public key authentication is necessary for all users.

<br>

### For Windows 10 _(build 1809 or later)_ and Windows 11

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
> **NO NEED** to overwrite the file. You can use the existing key-pair files to connect to the peacock server. A single key-pair can be used for multiple different remote servers without problem.

<br>

## Registering new user's SSH key _(as the admin)_

After the new user has generated a SSH key-pair, ask them to send the contents of their public key file _(eg. `~/.ssh/id_ed25519.pub` if they used default save path for Ed25519 key)_.

It should look something like this:

```
ssh-ed25519 AAAAC3NzaC...cHmsVu4vP5 myuser@LAPTOP-V66PNAGT
```

This string needs to be appended to the `.ssh/authorized_keys` file in their peacock server account's directory _(ie. `/home/[NEW_USER]/.ssh/authorized_keys` for a user created using `sudo adduser [NEW_USER]`)_.

As an admin logged in on the peacock server, you can do this using `echo` and `sudo tee`:

```bash
echo "ssh-ed25519 AAAAC3NzaC...cHmsVu4vP5 myuser@LAPTOP-V66PNAGT" | sudo tee -a /home/[NEW_USER]/.ssh/authorized_keys > /dev/null
```

<br>

## Accessing the server _(as the new user)_

For an account created via `sudo adduser [NEW_USER]`, the user can access the server via SSH by:

```bash
ssh [NEW_USER]@peacock.d2.comp.nus.edu.sg
```

> ⚠️ Access to SoC network is required, either by connecting to SoC wifi, [SoC VPN](https://dochub.comp.nus.edu.sg/cf/guides/network/vpn/start) or using [SoC SSH Jump Host](https://dochub.comp.nus.edu.sg/cf/guides/sjump/start).
