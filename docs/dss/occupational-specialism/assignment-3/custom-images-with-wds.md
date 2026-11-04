# Custom Images with WDS

### Guided Steps

Windows Deployment Services (WDS) includes a feature that allows a current Windows installation to be imaged and used on other machines. This means that an administrator can create an installation with a set of commonly used applications and files, and distribute it across other machines.

#### Part 1: Creating an image from an existing install.

1. Navigate to the Windows Deployment Menu in Windows Server. From Server Manager -> Tools -> Windows Deployment Services.

2. Navigate into your Server, then into the Boot Images folder in the side panel.

3. Right-click on your existing boot image file and click **Create Capture Image**.

4. Select a location to store your images. I recommend using the same location that is currently used for install images:

```powershell
	C:\RemoteInstall\Images\ImageGroup\NEWIMAGENAME.wim
```

5. Click **Next** to create the capture image.

6. Once the capture image has been created, ensure that you tick the box that states "Add image to the Windows Deployment Server now". Then, click "Finish".


![Add to WDS now](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/adds/AddtoWDSNow.png)



7. Click **Next** for the image location, it will then ask you to confirm the image name. Make sure that it's name is distinct from the standard Windows Boot Image.


You should now see your newly created capture image showing in your WDS boot images folder.

---
#### Part 2: Reference Windows Machine Installation

As part of your assessment, you will be provided with a Windows Server VM pre-configured with AD DS, DHCP, DNS, and WDS eliminating the need for you to set this up. However you will have to create your own Windows 10 reference machine that will be imaged. 

1. Create new Windows 10 VM that will server as the reference machine.
2. Install the software required by the assessment, this should be provided for you.

This system should be equipped standard desktop software as per the NCFE sample specifications:

* A browser that isn't Edge

* Word Processing

* VOIP / Messaging Software

* Antivirus Software

---
#### Part 3: Prepping for Capture:

1. Once all relevant software has been installed, we firstly need to prepare our system for imaging using sysprep.

2. Press **Windows + R** to open the run dialogue, then type in sysprep.

3. Right-click on sysprep.exe and run it as Administrator.


![Sysprep Location](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/SYSPREP.png)

4. From this interface, select the **Generalize** option, and change the action to **Shutdown** then click **Okay**
5. Your system will automatically power off once sysprep has completed.

---
#### Part 4: Preparing Networking Booting

Before undertaking this section, you will need to make adjustments to your boot options in Virtual Machine Manager so start by powering off your Windows 10 virtual machine as you will be making changes to its configuration.

1. Open the virtual machine window in Virtual Machine Manger and click on the LightBulb icon at the top-left corner of the window.

2. Select **Boot Options** from the left side panel.

3. Then, enable the boot menu and "NIC" as a bootable device.

4. Finally, click Apply

---
#### Part 5: Booting the Capture Image

1. When your virtual machine reboots, hit the **ESC** key when you see the "TianoCore" logo to enter the boot menu, from here head to boot manager, then select PXE IPV4. Your WDS server should be identified, press **Enter** to boot from it.

2. Select capture image from the boot menu.

![Windows Boot Manager](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/WBM.png)

3. From the Welcome page, click **Next**.

4. You can now select which Volume to Capture, by default this can be set to D:\

5. Give you image a name a description that will show in WDS, and to new machines that may use the image. Then click **Next**.

![Name Your Image](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/NameCaptureImage.png)

6. We can now set the destination for our new image. Set the **Name and Location** to an area on your Windows 10 in case of network failure.

7. Enter the IP of your server into the **Server name** field.

![Set Server Name/IP](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/SetServerName.png)

8. Click **Connect**, this should then open a terminal where you can login with your Windows Server Administrator account. It must begin with your AD DS Domain Name. e.g TLEVEL\Administrator.

![WDS Capture Login](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/CaptureLogin.png)

9. Once connected, you can select the ImageGroup to upload your capture image to upon complete of the imaging process.

10. Once selected, click **Next** to begin the capture process.

11. Once the imaging process is completed, click **Finish**. Your machine will then automatically reboot.

12. Return to your Windows Server virtual machine and re-open the **Windows Deployment Services** control panel. Check your **Install Images** folder, you should now see your custom capture image.


![Image confirmation](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/ImageConfirmation.png)

You can now use this image to create a duplicate installation of Windows by network booting a new machine.

---

#### Part 6: Installing With Your Image

Now that your custom image has been transferred to your WDS server, you can use it to install Windows with a custom set of applications, pretty nifty! 

1. In Virtual Machine Manager, create a new VM. Select **Manual Install**, then click **Forward**.
2. Set the operating system to **Microsoft Windows 10**, then click **Forward**. 
3. As usual, assign system resources and storage to the VM. 
4. Before finishing, click **Customise configuration before install**. 
5. Under **boot options** enable **NIC** and then move it up to the top of the list. Then click **Begin Installation**. 
6. When your VM boots, it will start the PXE network boot environment. When it detects your WDS Server, hit **F12** immediately to start WDS. 
7. You will be presented with the same screen as you were in part 5, step 2. Instead of selecting the Capture Image, this time, select the **Microsoft Windows Setup (x64)** option.

![WDS BootManager](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/WDSBootSetup.png)


8. This will boot into the Windows Deployment Services Installer, click **Next** to begin the installation.
9. Before the installation begins, you must log in to your Active Directory Administrator account. In this case: TLEVEL\Administrator and password. This allows your new VM to automatically join the AD Domain once the install is complete.

![AD Login](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/DomainLoginWDS.png)

10. You will then be presented with the Windows Install Image that you wish to use for this installation, from here you can use the image that you created. Note that your description also appears when the image is selected.

![Select Install Image](https://codeberg.org/RDMillen/ComputingEducation/raw/branch/main/images/wds/SelectInstallImage.png)

11. Complete the Windows installation as normal. Once your machine reboots, you will notice that any software that you added prior to running sysprep will be installed in this new installation!
