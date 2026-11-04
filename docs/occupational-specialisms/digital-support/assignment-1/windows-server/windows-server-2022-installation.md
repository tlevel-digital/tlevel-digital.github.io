# Windows Server 2022 Installation

### Guided Steps:

1. Setup a new virtual machine, select the Windows Server ISO file, allocate an appropriate amount of CPU cores and RAM.

2. Name your virtual machine and click "Finish"

3. Follow installation prompts, ensure that you select the **Desktop Experience** version. On the following page, accept the license and click "Next".

    > **Desktop Experience** gives the standard Windows GUI. While many things can be done without the GUI, some components of Windows Server require Desktop Experience, such as Windows Deployment Services and Transport (WDS).

4. Select **Custom** on the installation type menu, the select your virtual hard disk **Unallocated Space**.

5. Following a restart, to login, you will need to use Ctrl+Alt+Delete.

    - For **Virtualbox**, go to: Input, then Keyboard, and the select "Insert Ctrl-Alt-Delete".
    - For **Virtual Machine Manager (Linux)**, go to: "Send Key", then "Ctrl+Alt+Delete".
