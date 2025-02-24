# Creating new user on peacock server

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

Since OpenSSH is preinstalled on Windows 10 _(build 1809 or later)_ and Windows 11. Run these in CMD/PowerShell:

**CMD**:

```bash
ssh-keygen -t rsa -b 4096 -N "[PASSPHRASE]" -f "%USERPROFILE%\.ssh\[SAVE_FILE]"
```

**PowerShell**:

```bash
ssh-keygen -t rsa -b 4096 -N "[PASSPHRASE]" -f "$env:USERPROFILE\.ssh\[SAVE_FILE]"
```

**Interactive mode in either CMD or PowerShell** (prompts for inputs):

```bash
ssh-keygen -t rsa -b 4096
```

-   `[PASSPHRASE]` - is the passphrase to encrypt your SSH key with
-   `[SAVE_FILE]` - is the filename for the public-private key-pair

This will generate a public-private key-pair at these locations:

-   `C:\Users\[USERNAME]\.ssh\[SAVE_FILE]` - private key
-   `C:\Users\[USERNAME]\.ssh\[SAVE_FILE].pub` - public key

> ⚠️ **If `[SAVE_FILE]` exists already:** \
> It will prompt to overwrite the file. If overwritten, the old public-private key-pair at `[SAVE_FILE]` will be lost.

<br>

## Registering new user's SSH key _(as the admin)_

After the new user has generated a SSH key-pair, ask them to send the contents of their public key file _(ie. the `[SAVE_FILE].pub` from above)_.

It should look something like this:

```
ssh-rsa AAAAB3NzaC...HIbbDaTTAQ== shaun@LAPTOP-V66PNAGT
```

This string needs to be appended to the `.ssh/authorized_keys` file in their peacock server account's directory _(ie. `/home/[NEW_USER]/.ssh/authorized_keys` for a user created using `sudo adduser [NEW_USER]`)_.

As an admin logged in on the peacock server, you can do this using `echo` and `sudo tee`:

```bash
echo "ssh-rsa AAAAB3NzaC...HIbbDaTTAQ== shaun@LAPTOP-V66PNAGT" | sudo tee -a /home/[NEW_USER]/.ssh/authorized_keys > /dev/null
```

## Accessing the server _(as the new user)_

For an account created via `sudo adduser [NEW_USER]`, the user can access the server via SSH by:

```bash
ssh [NEW_USER]@peacock.d2.comp.nus.edu.sg
```

> ⚠️ Access to SoC network is required, either by connecting to SoC wifi, [SoC VPN](https://dochub.comp.nus.edu.sg/cf/guides/network/vpn/start) or using [SoC SSH Jump Host](https://dochub.comp.nus.edu.sg/cf/guides/sjump/start).
