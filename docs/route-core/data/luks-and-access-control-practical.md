# LUKS and Access Control Practical

In the following practical activity you install a Linux-based operating system and enable LUKS disk encryption as part of the installation process, linked to ISO 27001's requirement to secure data at rest.

*Data at rest* refers to data that is inactive/stored on a physical devices. For example, student work and credentials on a college PC that is turned off.

## Linux Install

For this task, you must use UEFI mode. When configuring your VM in virt-manager, before beginning an install select "Customise before install" and change *Firmware* setting from BIOS to UEFI, then click *Begin Installation*.

You may select an OS for this task, the vast majority of Linux distributions allow for full disk encryption as part of the partitioning options during installation. 

When installing Debian, unlike normal - set a root password, and REMEMBER it! We will set sudo up manually. Create an administrator user account like normal.

During installation of your OS, you will be about drive partitioning. Look for options relating to LUKS or "Encrypted LVM". You will be asked to setup a passphrase, then installation should continue as normal. 

Simply put, LUKS encrypts the contents of the main system drive and requires a master key to be entered by the user to unlock the drive and boot the operating system. 

When installing, remember to disable the GUI at the install level. If using Debian, uncheck the "Debian Desktop Environment" and "Gnome" check boxes, enable "SSH Server". 

## Sudo Configuration

Sudo is the privilege escalation program that is provided with almost all Linux distributions. There are two ways Linux can be configured:

1. The *root* and *user* accounts are both enabled on the system, when the user wishes to perform administrative tasks like add, removing, or updating packages they must log into the *root* account to perform these tasks.

2. Sudo is installed, allowing the user to execute *most* administrative tasks for their own user account. Similar to using the *Run as Administrator* option in Windows. This option is normally favoured as it limits what impact individual commands can have on the system.

For this section, we will be setting up three user accounts total.

* Our main administrator account
* A mid-level IT tech account
* An end user account

The main admin account will be allowed to execute any commands, the mid-level IT tech account can manage applications with apt, and check the status of system services, the third account cannot execute any commands that require administrator privileges.

Once rebooted, you can review the encrypted drive:

1. Identify your encrypted volume:

```bash
lsblk
```

2. Check details with cryptsetup:

```bash
cryptsetup status <name>
```

### 1. Installing Sudo

After installing your operating system, log into the `root` account that you set a password for during installation. If you set a root password during installation of Debian, the `sudo` package is not installed. 

1. Check for updates and install the sudo package. 

```bash
apt update && apt upgrade

apt install sudo -y
```
### 2.  Setting up user accounts

1. Create a new user group called `wheel`, then add your administrator account created during installation to the `wheel` group:

```bash
addgroup wheel
usermod -aG wheel administrator
```

2. Create your additional user accounts:

```bash
adduser moss

adduser jen
```


3. Next, we will configure the sudo configuration file to adjust what they can/cannot do. 

```bash
visudo
```

4. Add the following line to allow all members of the `wheel` user group to run administrative commands:

```bash
%wheel      ALL=(ALL:ALL) ALL
```

5. Add a line to the end of this file in the following format: 

```bash
moss ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl status *
```

The above line states the the user `moss` can run programs `apt` and `systemctl status` using sudo, but nothing else. 

### 3. Test

Log into the various user accounts and test the commands that have been configured, notice any interesting error messages?
