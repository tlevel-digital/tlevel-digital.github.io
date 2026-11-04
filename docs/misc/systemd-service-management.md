# SystemD Service Management

Some applications have a background service or **daemon** that can be configured to run at boot, or manually started when needed. For this example, I will be using `libvirtd`, the virtualisation service.

## Service Management

Services are controlled using the `systemctl` command, as this is a system administration command, you will need to use `sudo` as well. 

There are five main options of systemctl that we will look at:

* `status` - Check the status of the service.
* `start` - Start the service once.
* `stop` - Stop the service.
* `enable` - Enable the service to run at boot time.
* `disable` - Disable the service from running at boot time.

The syntax for systemctl is:

```bash
sudo systemctl [OPTION] [ServiceName]
```

### Status

To check if a service is currently running, view service log files, or to see if a service has crashed you can use the status flag:

```bash
sudo systemctl status libvirtd
```

If a service is not behaving as expected, this is a good starting place for troubleshooting.
### Start

For example, once you have installed `libvirtd` it must be started before you can run a virtual machine:

```bash
sudo systemctl start libvirtd
```

### Stop

To stop a currently running service:

```bash
sudo systemctl stop libvirtd
```

### Enable

If you use virtual machines regularly, having this service start at boot would be a good idea:

```bash
sudo systemctl enable libvirtd
```

*Note:* Enable only sets the service to start on the next reboot, if you want to both start the service AND have it run at boot, you can use the --now flag:

```bash
sudo systemctl enable --now libvirtd
```

### Disable

If you no longer want a service to start a boot time, use the disable command:

```bash
sudo systemctl disable libvirtd
```

*Note:* This does not stop the service, it only disables it from starting at boot. 

## Additional Information

The majority of mainstream Linux distributions use SystemD as the service manager for the operating, so these commands will work on most OSes. Including: Ubuntu, Debian, Arch Linux, SteamOS, Linux Mint, and many more.

For more advanced systemctl command, there is a great article on DigitalOcean:

[Digital Ocean Managing Service Guide](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)
