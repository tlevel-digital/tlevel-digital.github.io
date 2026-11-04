# Installing Debian Linux for Server Use

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
