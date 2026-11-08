# SQLite Database on Debian Server

In this session we will be creating a new VM on Proxmox to host our SQLite database, then allow remote access to this database for viewing and management. 

**Reminder:** When setting up VMs for a practical session, make sure that you are distributing yourselves across the virtualisation servers to improve performance.

To support with this activity, the below pages may be of use:

* [Linux Package Management](../../misc/linux-package-management.md)
* [The GNU Nano Text Editor](../../misc/the-gnu-nano-text-editor.md)

## Proxmox & Debian Prep

### 1. Create a Debian VM in Proxmox

1. In Proxmox Web UI:
2. Click **Create VM**.  
3. **General:** Give it a name (e.g. `debian-sqlite`).  
4. **OS:** Choose the Debian ISO. 
5. **System:** Default.  
6. **Hard Disk:** 10–20 GB qcow2, SCSI.  
7. **CPU:** 1–2 cores.  
8. **Memory:** 2 GB.  
9. **Network:** VirtIO bridge to your lab LAN.  

### 2. Install Debian

1. Boot the VM → “Graphical Install”.  
2. Choose **English / UK locale / UK keyboard**.  
3. Hostname: your-server-name.  
4. Domain: leave blank.  
5. Leave the root password blank!.  
6. Create a student user (e.g. `student`).  
7. Partition: “Guided – use entire disk”.  
8. Software selection: Uncheck **Debian Desktop Environment AND Gnome**, select only:  
    - `SSH server`  
    - `standard system utilities`.  
9. Install GRUB → Yes → continue.  

At reboot you’ll have a CLI-only Debian server.

### 3. Prepare the VM

1. Log in as  `YourUserAccount`.  
2. Update system:  

    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

4. Test SSH from a lab workstation:  

    ```bash
    ssh username@<vm-ip>
    ```

At this point the VM is ready for students to log in and follow their worksheet.  

---

## SQLite Database Setup

### 1. Transfer the Database File

You can access the `.db` file in Teams. (e.g. `dbMusic.db`).  

1. From your workstation terminal (not inside VM):  

    ```bash
    scp dbMusic.db <your-username>@<vm-ip>:/home/your-username/
    ```

2. Back in your VM, check the file:  

    ```bash
    ls -l
    ```

You should see `dbMusic.db`.  

---

### 2. Install SQLite

1. Install SQLite:  

    ```bash
    sudo apt install sqlite3 sqlite3-tools
    ```

2. Open the database:  

    ```bash
    sqlite3 dbMusic.db
    ```

    Inside SQLite:  

    ```sql
    .tables
    SELECT * FROM tblTracks LIMIT 5;
    .quit
    ```

You should be able to view existing data.  

---

### 4. Install SQLite-Web

1. Install Python + pip:  

    ```bash
    sudo apt install python3-pip python3-venv pipx
    ```

2. Install SQLite-Web:  

    ```bash
    pipx install sqlite-web
    pipx ensurepath
    ```

3. Once you have installed sqlite-web, you will need to log out and back into your VM session.

4. Run against your DB:  

    ```bash
    sqlite_web --host 0.0.0.0 --port 8080 dbMusic.db
    ```

---

### 5. Access from a Browser

1. Check VM IP:  

    ```bash
    ip a
    ```

    Example: `192.168.1.45`.  

2. On your lab workstation browser:  

    ```
    http://192.168.1.45:8080
    ```

3. You should see SQLite Web UI.  

Your database should be visible in your browser.

---

### 6. Extensions

- Run `sqlite_web --host 0.0.0.0 --port 8080 new.db` to host a fresh database.  
- Add a new table in the web UI.  .  

---

## Troubleshooting & Notes

### Common Issues

- **SSH refused**: Attempt to `ping` your VM to check for connectivity.
- **scp fails**: Must be run on your host machine, not within SSH - run from your workstation.  
- **pip error**: Run `pip3 install --upgrade pip` first.  

### Stretch/Extensions

- Create a **systemd service** for SQLite-Web so it runs on boot.  

- Add a simple firewall rule with `ufw`:  

    ```bash
    sudo apt install ufw
    sudo ufw allow ssh 
    sudo ufw allow 8080/tcp
    sudo ufw enable
    ```

- SQLite is considered a quick an easy solution for small scale databases. Enterprise solutions are normally handled by either MySQL/MariaDB, PostgreSQL, or MongosDB. once you have completed the above tasks, attempt to setup and configure MariaDB and PHPMyAdmin using the following guidance: [MariaDB and PHPMyAdmin on Debian](https://phoenixnap.com/kb/how-to-install-phpmyadmin-on-debian).
- To learn more about SQL commands, I recommend [Codecademy's Intro to SQL](https://www.codecademy.com/learn/intro-to-sql)
### Reflection Questions

1. What is the difference between accessing SQLite locally vs through a web service?  
2. Why is `scp` more secure than copying files with a USB stick or shared folder?  
3. What extra steps would be needed if this was a **production database**? (Think: multiple users, security, backups).  
