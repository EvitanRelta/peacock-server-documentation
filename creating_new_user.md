# Creating new user on peacock server

This guide is on creating a new account on the peacock server and giving a new user access to the account.

<br>

## Step 1: Create new user account _(as the admin)_

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

## Step 2: Generating SSH key-pair _(as the new user)_

The peacock server has password login disabled, so SSH public key authentication is necessary for all users.

Refer to [`docs/generating_ssh_key_pair.md`](docs/generating_ssh_key_pair.md) for a guide on how to generate a SSH key-pair.

<br>

## Step 3: Registering new user's SSH key _(as the admin)_

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

## Step 4: Accessing the server _(as the new user)_

For an account created via `sudo adduser [NEW_USER]`, the user can access the server via SSH by:

```bash
ssh [NEW_USER]@peacock.d2.comp.nus.edu.sg
```

> ⚠️ Access to SoC network is required, either by connecting to SoC wifi, [SoC VPN](accessing_outside_soc.md#soc-vpn) or using [SoC SSH Jump Host](accessing_outside_soc.md#soc-ssh-jump-host).

<br>

## Step 5: Granting admin permissions _(as the admin)_

To give the new user admin privileges, add them to the `sudo` group:

```bash
sudo usermod -aG sudo [NEW_USER]
```

This grants the user the ability to:

-   Use the `sudo` prefix to execute commands with root privileges
-   Create new users and manage user permissions _(using `sudo adduser`)_
-   Grant admin privileges to other users

Users will need to re-login for this change to take effect.
