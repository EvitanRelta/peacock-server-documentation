# Accessing server outside of SoC

This guide is on accessing the peacock server outside of SoC.

The peacock server, like all other SoC clusters, requires access to the SoC network either by connecting to SoC wifi, [SoC VPN](https://dochub.comp.nus.edu.sg/cf/guides/network/vpn/start) or using [SoC SSH Jump Host](https://dochub.comp.nus.edu.sg/cf/guides/sjump/start).

Without access to the SoC network, the server would not be discoverable to you:

```
# In CMD:
C:\Users\myuser> ssh myuser@peacock.d2.comp.nus.edu.sg
ssh: connect to host peacock.d2.comp.nus.edu.sg port 22: Connection timed out
```

Below are instructions on how to setup and use SoC VPN / SSH Jump Host.

<br>

## SoC VPN

Follow the official SoC DocHub guide to setup FortiClient VPN for SoC VPN: https://dochub.comp.nus.edu.sg/cf/guides/network/vpn/start

Once you've set FortiClient up, you'll need to boot up FortiClient and connect to the SoC VPN first before you're able to SSH into SoC servers.

<br>

## SoC SSH Jump Host

An alternative to using SoC VPN.

This is based on the official SoC DocHub guide on SSH Jump Host is at: https://dochub.comp.nus.edu.sg/cf/guides/sjump/start

<br>

### Apply for an SoC account

If you're a SoC student but don't have an SoC account yet, you should be above to apply for one at: https://mysoc.nus.edu.sg/~newacct/

<br>

### Generating SSH key-pair

Refer to [`docs/generating_ssh_key_pair.md`](docs/generating_ssh_key_pair.md) for a guide on how to generate a SSH key-pair.

<br>

### Register your SSH key via SSH Keys service

This is based on the official SoC DocHub guide on SSH Jump Host is at: https://dochub.comp.nus.edu.sg/cf/guides/skeys

1.  Connect to SoC wifi. Or if you're not in SoC, connect to [SoC VPN](#soc-vpn)

1.  Add SSH public key to `skeys.comp.nus.edu.sg` server

    -   **For Windows:**

        > ⚠️ The `skeys.comp.nus.edu.sg` requires sending a Unix EOF signal _(by pressing `Ctrl+D`)_ to finish your input, but Window's CMD/PowerShell terminals are unable to do that. Thus, you'll have workaround using `Get-Content`, `type` or Git Bash instead, as detailed below.

        Assuming your SSH public key is at `C:\Users\[USERNAME]\.ssh\id_ed25519.pub`, run this command in either one of these terminals, and login with your SoC account's password:

        -   **CMD:**

            ```
            type C:\Users\[USERNAME]\.ssh\id_ed25519.pub | ssh [SOC_ACC_USERNAME]@skeys.comp.nus.edu.sg jump
            ```

        -   **PowerShell:**

            ```
            Get-Content C:\Users\[USERNAME]\.ssh\id_ed25519.pub | ssh [SOC_ACC_USERNAME]@skeys.comp.nus.edu.sg jump
            ```

        -   **Git Bash (or any Unix-like terminals):**

            ```
            ssh [SOC_ACC_USERNAME]@skeys.comp.nus.edu.sg jump

            # login, paste the contents of id_ed25519.pub, then Ctrl + D
            ```

            **Example usage:**

            ```
            $ ssh myuser@skeys.comp.nus.edu.sg jump
            myuser@skeys.comp.nus.edu.sg's password:
            Could not chdir to home directory /home/s/myuser: No such file or directory
            ssh-ed25519 AAAAC3NzaC...cHmsVu4vP5 myuser@LAPTOP-V66PNAGT
            OK: Authorised keys updated for jump access. Key fingerprints are:
            256 SHA256:UQiyMbYdKm9S9WaXxxXkms2mCyQZFcUmB7nrA+l/qv8 (ED25519)
            ```

> ⚠️ Running the `ssh ___@skeys.comp.nus.edu.sg jump` command again will overwrite the previous config, and leaving the input blank will remove all authorised keys.

<br>

### Using the SSH Jump Host

To connect to this peacock server:

```bash
ssh peacockuser@peacock.d2.comp.nus.edu.sg
```

... without SoC wifi or SoC VPN, you can route it through the SoC Jump Host by:

```bash
ssh -J [SOC_ACC_USERNAME]@sjump.comp.nus.edu.sg peacockuser@peacock.d2.comp.nus.edu.sg
```

... and login to the `sjump` server using your SoC account's password.
