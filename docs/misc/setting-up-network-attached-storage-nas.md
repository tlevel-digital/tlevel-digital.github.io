# Setting up Network Attached Storage (NAS)

In this session we will:

* Install Debian Linux inside of a virtual machine (VM)
* Install file sharing software
* Make the folder accessible on the network
* Restrict access to the folder to certain users

For this session we will be utilising the Proxmox, a type 1 hypervisor, then creating a new virtual machine to host our files. 

Guidance on accessing Proxmox is available here: [Using Proxmox Virtualisation Servers](using-proxmox-virtualisation-servers.md)

---

## Step 1 - Setting up a VM in Proxmox

1. From the main interface of Proxmox, select *Create VM* from the top right-corner of the interface to open the creation interface.
2. Give your VM a name in the *Name* field. This should be unique to your machine, then click *Next*.

![Screenshot](../assets/images/pasted-image-20251111200520.png)

3. Next we will select our installation source, from the ISO Images drop down, select debian-13. Then click *Next*.

![Screenshot](../assets/images/pasted-image-20251111200549.png)

4. Leave the system settings at default, click *Next*.
5. Now we allocate storage for our virtual machine, we will leave this at the default 32GB. Click *Next*.

![Screenshot](../assets/images/pasted-image-20251111200638.png)

6. Now we can choose how many processor cores we are going to allocate our VM. Increase to two cores, then click *Next*.

![Screenshot](../assets/images/pasted-image-20251111200718.png)

7. Next is RAM allocation, we are going to provide our machine with 2GB. In this case we must provide it in megabytes - enter 2048, then click *Next*.

![Screenshot](../assets/images/pasted-image-20251111200729.png)

8. Leave all settings on the network tab as default, click *Next*.
9. Once you have verified the details, click *Finish*.

Your VM is now configured and ready to be started. You should see your VM appear in the *Server View*. 

## Step 2 - First Boot and Install 

Now that the operating system is configured and ready, we are going to boot up the virtual machine and start the installation process. 

1. In the Virtual Machine area, click the *Console* tab. This is our virtual screen for the VM. 
2. To start the VM click *Start* at the top-left of the Virtual Machine area. 
3. The machine will now boot from the install image. 
4. When prompted, press enter to select *Graphical Installer*. 

![Screenshot](../assets/images/pasted-image-20251111200920.png)

The virtual machine will now boot the guided installer for Debian. 

1. Select *English* and then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111200946.png)

2. Select *United Kingdom* as your location, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201000.png)

3. Select *British English*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201012.png)

4. Next we will choose our machine's *hostname*. This must be unique to your machine, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201131.png)

5. Leave the *Domain name* field empty, then click *Continue*.
6. Leave the root password fields empty, then click *Continue*.
7. Enter the full name of your user, for example "Bruce Wayne", then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201210.png)

8. The username should be automatically filled in with your first name. Usernames must always be lowercase. I recommend not changing this, but remember it for logging in! Click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201230.png)

9. Set a password for your user account, confirm it, and don't forget it! Something simple like *Password* is good for testing here. Then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201252.png)

10. Next, we will be setting up our storage drive, click *Guided - use entire disk*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201315.png)

11. Select the available 32GB storage disk (this may show as 34.4GB due to some VM jiggery-pokery), then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201328.png)

12. Select *All file in one partition (recommended for new users)*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201417.png)

13. Select *Finish partitioning and write changes to disk*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201433.png)

14. Under *Write the changes to disks*, select *Yes*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201444.png)


The installer will now start installing the base system without an additional packages, we will customise our installation in the next few menus. 

1. When asked *Scan extra installation media*, leave this set to *No*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201617.png)

2. For the mirror (server hosting a copy of the installation files), select *United Kingdom*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201626.png)

3. When asked to select a web address, select *deb.debian.org*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201645.png)

4. Leave *HTTP proxy information* blank, then click *Continue*.
5. When prompted whether to *Participate in the package usage survey* ,select *No*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111201756.png)

6. When presented with different package options for installation, you will need to adjust what Debian is going to install to look like this - disable *Debian desktop environment* and *... GNOME*, then enable *SSH Server*, and *standard system utilities*. Then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111130050.png)

7. You will now be prompted to install the *boot loader* this is the piece of software that "starts" the operating system up. When prompted to *Install the GRUB Boot loader to your primary drive*, click *Yes*, then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111202045.png)

8. Select your primary drive, in this case the device labelled */dev/sda*. Then click *Continue*.

![Screenshot](../assets/images/pasted-image-20251111202125.png)


9. Once the boot loader is installed, you are now ready to reboot the system into the newly-installed operating system! Click *Continue* to reboot!

![Screenshot](../assets/images/pasted-image-20251111202320.png)

Upon reboot you will be greeted with a prompt that looks like this - ready for you to login! 

![Screenshot](../assets/images/pasted-image-20251111202440.png)

## Step 3 - Logging In and Basic Commands

Firstly, we need to get logged in using the username and password that you created during the installation process, when you type in your password no characters will appear. This is a security measure to prevent people from being able to see how long your password is! 

```bash
DebianVM login: ross
Password: 
```

You should then be greeted with another blank screen!

```bash
ross@DebianVM:~$ |
```

Now that we are logged in, we need to get the IP address of our VM so that we can remotely connect to it from our lab workstation.

1. Run the `ip a` command to get a detailed output of your system's addresses.

![Screenshot](../assets/images/pasted-image-20251111213650.png)

Our interface is named *ens18*, our IP address is displayed in pink. 

2. On your lab workstation, open the *Konsole* application. This is how we access the command line to issue commands on our local machine.

![Screenshot](../assets/images/pasted-image-20251111214108.png)

3. Next, we will use the `ssh` tool to connect to our remote virtual machine using the following command: `ssh username@ip-address`. My username in this example is `bruce` and my IP address is `192.168.0.167`.

![Screenshot](../assets/images/pasted-image-20251111214345.png)

4. You will be prompted whether or not you wish to continue connecting, enter *yes* and press *Enter*.

![Screenshot](../assets/images/pasted-image-20251111214427.png)

5. When prompted, enter the password for the user account you created for your VM:

![Screenshot](../assets/images/pasted-image-20251111214519.png)

6. Success! You should now be connected to your VM, notice how the username and hostname on the command line have changed: 

![Screenshot](../assets/images/pasted-image-20251111214554.png)

### Basic Commands

Here are some basic commands that are useful across practicals:

| Command | Function                                                     |
| ------- | ------------------------------------------------------------ |
| ls      | List the content of the current directory.                   |
| cd      | **C**hange **d**irectory, use to move into another folder.   |
| cat     | Print the contents of a file to the terminal.                |
| cp      | Copy a file to another location - `cp SOURCE DESTINATION`' . |
| mv      | Move a file to another location - `mv SOURCE DESTINATION`.   |
| ip a    | List network interfaces, IP addresses, and MAC addresses.    |

## Step 4 - Installing and Configuring Samba

In order to share a folder on the network, we use a piece of software called *Samba*. For this we will need to install some new packages and setup some services on our VM. For this section, we will be using the `sudo` command, this is the equivalent of *Run as Administrator* on Windows. You will be prompted for your password, as before it will not show any characters or asterisks as you type.

This section also involves editing files using a command-line text editor, more information on the `nano` editor can be found here: [The GNU Nano Text Editor](the-gnu-nano-text-editor.md)


1. First, let's create the folder that we want to share on the network. This folder is going to be called *Batshare*. It is best practice for shared folders to be stored in the `/srv` folder on Linux.

```bash
sudo mkdir /srv/Batshare
```

2. Now we give ownership of the folder to our user using the following command:

```bash
sudo chown -R bruce:bruce /srv/Batshare
```

3. Before we install, let's ensure our operating system is up to date:

```bash
sudo apt update

sudo apt upgrade
```

2. Next, we will install all of the packages that we are going to need to run Samba, press *Y* when prompted to install.

```bash
sudo apt install samba smbclient
```

![Screenshot](../assets/images/pasted-image-20251111214841.png)

3. Now we will configure Samba using `nano`:

```bash
sudo nano /etc/samba/smb.conf
```

![Screenshot](../assets/images/pasted-image-20251111214942.png)

4. This will open the Samba configuration file, there is a lot of information in here! We only need to make some additions to the end of the file. Scroll down to the bottom of the file, you can use either arrow keys or PgUp and PgDown to navigate around the file.

![Screenshot](../assets/images/pasted-image-20251111215012.png)

5.  Add the following details to your `smb.conf` file:

```bash
[ShareName]
	comment = Enter your comment here
	read only = no
	path = /srv/YOURFOLDER
	guest ok = no
```

![Screenshot](../assets/images/pasted-image-20251111215844.png)

6. Once you have finished editing your `smb.conf` file, press Ctrl+X, then Y, then Enter to save changes. 
7. Samba uses it's own username and password system, we need to add tell Samba to set a password for our user. We will use these details to access our share later. 

```bash
sudo smbpasswd -a bruce
```

7. When prompted, enter a password. I recommend keeping it the same as your main login for ease - in practice different passwords are always the best options.

![Screenshot](../assets/images/pasted-image-20251111220149.png)

8. Now that everything is configured, we will start the Samba service and configure it to start automatically if the VM is rebooted.

```bash
sudo systemctl start smbd

sudo systemctl enable smbd
```

### Testing the Share

1. Open the *Dolphin* file manager:

![Screenshot](../assets/images/pasted-image-20251111221536.png)

2. Navigate to the *Network* tab:

![Screenshot](../assets/images/pasted-image-20251111221621.png)

3. In the address bar at the top of the window type in the following address:

```http
smb://YOUR-IP/ShareName
```

For my example, the address will look like this: 

![Screenshot](../assets/images/pasted-image-20251111221755.png)

4. Enter your username and password that you configured during the `smbpasswd` setup.

![Screenshot](../assets/images/pasted-image-20251111221834.png)

5. You should now be connected to your Network Attached Storage! Huzzah! 

### Verify Function

Now that we are connected, we can test to make sure that everything is working correctly. 

1. In the *Dolphin* file manager, create some test files and folders:

![Screenshot](../assets/images/pasted-image-20251111222310.png)

2. In your *Konsole* window that is also connected to your server, enter the following command:

```bash
ls -l /srv/Batshare
```

![Screenshot](../assets/images/pasted-image-20251111222439.png)

3. We can also check the contents of sub-folders by adding the folder name to the end of the `ls -l` command:

```bash
ls -l /srv/Batshare/Bat\ Finances/
```

![Screenshot](../assets/images/pasted-image-20251111222652.png)

4. We can use `nano` to open the text files in the same way that we edited the `smb.conf` file:

```bash
nano /srv/Batshare/Bat\ Spaghetti\ Recipie.txt
```

![Screenshot](../assets/images/pasted-image-20251111222829.png)

## Step 5 - Create a public share

The share that we have just created is restricted so that only your user can access it, create a second folder to share on the network. This time, the folder must be accessible to anyone to read/write to without authentication. 

There are two pieces of guidance you will need to get this work:

1. When you create your folder to share, instead of giving a user ownership of the folder, you must set it so that anyone can read and write to it using the following command:

```bash
sudo chmod -R 777 /srv/PublicShare
```

2. When editing your `smb.conf` file, there are three additional lines that need to be added to the share definition alongside the parameters used before:

```bash
public = yes
guest ok = yes
writable = yes
```

Once you have made changes to the `smb.conf` file, you will need to reload the Samba service:

```bash
sudo systemctl restart smbd
```

Have another member of the class test if they can access your share.
