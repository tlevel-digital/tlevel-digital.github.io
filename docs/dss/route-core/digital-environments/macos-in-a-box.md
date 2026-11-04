# MacOS in a Box

Naturally at some point in your IT career, you will encounter a wild Macintosh device within a Windows environment. This provides many challenges for you as a budding IT engineer. 

As we are not made of Mac devices, we will be using a virtualised instance of MacOS and look at it's integration with Windows Services like Active Directory and Samba network file systems.

## Installation of a MacOS VM

For this we will be using a piece of software called `quickemu`:

1. Pull the repo from Github:

```bash
git clone --filter=blob:none https://github.com/quickemu-project/quickemu
```

2. Change into the downloaded directory:

```bash
cd quickemu/docs
```

3. Install the project:

```bash
sudo make install
```

4. We will now use `quickemu` to download the required installation files for MacOS Sonoma and start the installation process.

```bash
quickget macos sonoma
```

5. Boot up the VM:

```bash
quickemu --vm macos-sonoma.conf
```

6. Use cursor keys and enter key to select the macOS Base System
7. From macOS Utilities, Click Disk Utility and Continue.
8. Select `QEMU HARDDISK Media` (~103.08GB) from the list (`Apple Inc. VirtIO Block Device`) and click Erase.
9. Enter a `Name:` for the disk, Click Erase, then Click Done.
10. From macOS Utilities click Reinstall macOS and Continue
11. Complete the installation, on the first reboot use cursor keys and enter key to select macOS Installer. On the subsequent reboots use cursor keys and enter key to select the disk you named

## Self-directed

Utilising your existing knowledge of Windows Server, create a VM with AD DS running. Attempt to integrate your MacOS installation with your newly created Active Directory provision.

* [Apple's AD DS Integration Guidance](https://support.apple.com/en-gb/guide/directory-utility/diru39a25fa2/mac)

### Notes

* `quickemu` has some software dependencies that do not actively get pulled in when installed from Github. If for any reason it does not work, here are the full list of dependencies:
    * [QEMU](https://www.qemu.org/) (_6.0.0 or newer_) **with GTK, SDL, SPICE & VirtFS support**
    - [bash](https://www.gnu.org/software/bash/) (_4.0 or newer_)
    - [Coreutils](https://www.gnu.org/software/coreutils/)
    - [curl](https://curl.se/)
    - [EDK II](https://github.com/tianocore/edk2)
    - [gawk](https://www.gnu.org/software/gawk/)
    - [grep](https://www.gnu.org/software/grep/)
    - [glxinfo](https://gitlab.freedesktop.org/mesa/demos)
    - [jq](https://stedolan.github.io/jq/)
    - [pciutils](https://github.com/pciutils/pciutils)
    - [procps](https://gitlab.com/procps-ng/procps)
    - [python3](https://www.python.org/)
    - [mkisofs](http://cdrtools.sourceforge.net/private/cdrecord.html)
    - [usbutils](https://github.com/gregkh/usbutils)
    - [util-linux](https://github.com/karelzak/util-linux); including `uuidgen`.
    - [sed](https://www.gnu.org/software/sed/)
    - [socat](http://www.dest-unreach.org/socat/)
    - [spicy](https://gitlab.freedesktop.org/spice/spice-gtk)
    - [swtpm](https://github.com/stefanberger/swtpm)
    - [xdg-user-dirs](https://www.freedesktop.org/wiki/Software/xdg-user-dirs/)
    - [xrandr](https://gitlab.freedesktop.org/xorg/app/xrandr)
    - [zsync](http://zsync.moria.org.uk/)
    - [unzip](http://www.info-zip.org/UnZip.html)
