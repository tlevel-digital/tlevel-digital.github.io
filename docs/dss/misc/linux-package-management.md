# Linux Package Management

In the following guidance, you will find information on managing programs Debian, Fedora, and SUSE-based Linux distributions. 

## Package Management

Each distribution of Linux uses a different command-line application for the management of software. 

* `zypper` for OpenSUSE Tumbleweed
* `apt` for Debian and Ubuntu Linux
* `dnf` for Fedora, Rocky, Alma, and Red Hat Enterprise Linux

As the package manager modifies programs on the system, it must be run using the `sudo` command. 
### System Update

In order to update your system, issue the following command in your terminal.

* Tumbleweed - `sudo zypper dup`
* Debian - `sudo apt update && sudo apt upgrade`
* Fedora - `sudo dnf update`

This automatically checks for latest updates for both the operating system *and* your installed packages.

### Searching For Programs

When looking for a specific piece of software, you can search for it using the package manager:

* Tumbleweed - `sudo zypper search PROGRAM`
* Debian - `sudo apt search PROGRAM`
* Fedora - `sudo dnf search PROGRAM`

This command will return the exact package name for the program that you are looking for, allowing you to the install it.

### Software Installation

Once you have the name of the package(s) that you wish to install:

* Tumbleweed - `sudo zypper in PACKAGE`
* Debian - `sudo apt install PACKAGE`
* Fedora - `sudo dnf install PACKAGE`

For more than one package, you can list all packages separated by a space.
### Software Removal

If you wish to uninstall previously installed software:

* Tumbleweed - `sudo zypper remove PACKAGE`
* Debian - `sudo apt remove PACKAGE`
* Fedora - `sudo dnf remove PACKAGE`

Like the install command, you can remove multiple packages at a time by separating them with a space.

*Caution:* Linux is very trusting of its user and will willing allow you to remove critical packages and files that can potentially break your system. Be careful!
