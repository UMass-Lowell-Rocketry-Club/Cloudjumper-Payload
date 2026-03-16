# Payload Raspberry Pi Config

These are the steps taken to set up the Payload Raspberry Pi.

## Device Specifications

| Name    | Specification          |
| ------- | ---------------------- |
| Device  | Raspberry Pi Zero 2 W  |
| RAM     | 512 MB                 |
| Storage | 64 GB SanDisk microSD  |
| OS      | Raspberry Pi OS Lite   |

## Initial Setup

The steps to set up the Raspberry Pi in a usable state for further work.

### OS Installation

Install **Raspberry Pi OS Lite** on the microSD card using [Raspberry Pi Imager](https://www.raspberrypi.com/software/).

- OS: **Raspberry Pi OS Lite (64-bit)**
- Storage: **64 GB SanDisk microSD**
- Hostname: `rocketryslice0` (connect via `[user]@rocketryslice1.local`)
  - *Other Pi's have subsequently numbered hostnames.*
- **Localisation:**
  - Capital city: **Washington, D.C. (United States)**
  - Time zone: **America/New_York**
  - Keyboard layout: **us**
- **Choose username:**
  - Username: `rocketry`
  - Password: *See Principle Software Lead for access*
- **Choose Wi-Fi:**
  - SSID: `UMassLowell`
    - *Select "Open Network"*
    - *More details in [Network Access](#network-access).*
- **SSH authentication:**
  - Enable SSH: **Yes**
  - Authentication mechanism: **Use password authentication**
- **Raspberry Pi Connect:**
  - Enable Raspberry Pi Connect: **No**

1. One data writing is complete, take the microSD card out of the formatting computer.
2. With the Raspberry Pi disconnected from power, insert the microSD card into the Pi's microSD card slot, next to the mini-HDMI port.
3. Connect the Pi to power. Boot up may take a few minutes.

### Network Access

*TODO: Add network access instructions.*

## Settings

### Settings from CLI

Type `sudo raspi-config` to enter the **Raspberry Pi Software Configuration Tool**.

- **System Options**
  - Auto Login: **No**
  - Splash Screen: **Yes**
- **Interface Options**
  - I2C: **Yes**

```bash
git config --global user.name "rocketry"
git config --global user.email "rocketry@rocketrypie.local"
```

### Config File Customization

**`~/.bashrc`:**

Uncomment or add the following lines:

```bash
GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'
alias ll='ls -l'
alias la='ls -A'
alias l='ls -CF'
```

**`~/.bash_aliases`:**

*`.bashrc` includes a line to evaluate `.bash_aliases` as well and recommends placing aliases in `.bash_aliases`.*

```bash
# Alias to reload the default config file in the current terminal session
alias reload="source ~/.bashrc"

# Alias to clear the screen with the command 'c'
alias c="clear"

# Alias to run eza (better ls)
alias le="eza -l"
```

**Sudo No-Password Fix:**

This causes sudo to enforce password verification. Credit to [ripat](https://forums.raspberrypi.com/viewtopic.php?t=97334#p676504) for the solution.

```bash
sudo usermod -a -G sudo rocketry # add rocketry to the sudo group
sudo visudo /etc/sudoers.d/010_pi-nopasswd
# COMMENT OUT THE FOLLOWING LINE
rocketry ALL=(ALL) NOPASSWD: ALL
sudo visudo /etc/sudoers
# MAKE SURE THE FOLLOWING LINE IS PRESENT
%sudo    ALL=(ALL:ALL) ALL
```

## Installed Software

---

```bash
# git (most popular version control system)
sudo apt install git

# cmake (multiplatform build tool)
sudo apt install cmake

# GitHub CLI (CLI for managing repositories on GitHub)
sudo apt install gh

# fastfetch (tool to display system logo and info)
sudo apt install fastfetch

# eza (better ls)
sudo apt install eza

# speedtest-cli (internet speed test cli)
sudo apt install speedtest-cli
```

## Remote SSH Access

### SSH Daemon Config

The main **sshd** config file is `/etc/ssh/sshd_config`. This could be edited directly, but it includes a line at the top, `Include /etc/ssh/sshd_config.d/*.conf`, which means any file in `/etc/ssh/sshd_config.d/` with the extension `.conf` will be evaluated first. As such, all sshd config options are stored in **`/etc/ssh/sshd_config.d/sshd_custom_config.conf`:**

```bash
# This file is fully written for the rocketry club pi with help from the following sources:
# - https://www.ssh.com/academy/ssh/sshd_config
# - https://www.raspberrypi.com/documentation/computers/remote-access.html
# - https://www.howtogeek.com/devops/what-is-ssh-agent-forwarding-and-how-do-you-use-it/

# Disable X11 Forwarding, which is a way of running graphical interfaces over
# ssh. After testing it myself, it seems to work some of the time and not other
# times, and is mostly depreciated.
X11Forwarding no

# Enable ssh agent forwarding, which allows ssh keys stored through the user's
# ssh agent to be securely used on the server, when the user chooses to do so
AllowAgentForwarding yes

# Prohibit ssh login as root
PermitRootLogin no
```
