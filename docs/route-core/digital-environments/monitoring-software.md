# Monitoring Software

Enterprise environments will generally use a form of monitoring software to both assess performance and monitoring reliability of network nodes within their infrastructure. This process can be as simple as a piece of software pinging a node to check whether its status is *up* or *down*, or full detailed diagnostics checking CPU, RAM, storage, and network metrics. 

We will be utilising both of these types of monitoring for our own dashboard software. This will be utilising [Grafana](https://grafana.com/) and [Homepage](https://gethomepage.dev/).

Monitoring software consists of two parts, the logging server application and the client reporting/collection application. 

* *Client*: Reports metrics and data logged.
* *Server*: Stores the logged data, making it accessible to present through a piece of dashboard software.
## Grafana Client Setup

The reporting application for Grafana is called *Alloy* as is available for Windows, MacOS, and Linux. For OpenSUSE Tumbleweed follow the commands below:

1. Import the encryption key to verify the repository:

```bash
wget -q -O gpg.key https://rpm.grafana.com/gpg.key
```

2. Import the encryption key into the package manager:

```bash
sudo rpm --import gpg.key
```

3. Add the Grafana repository:

```bash
sudo zypper addrepo https://rpm.grafana.com grafana
```

4. Sync up your repositories:

```bash
sudo zypper update
```

5. Install Alloy using Zypper:

```bash
sudo zypper install alloy
```

## Grafana Client Configuration

*TBC*

## Grafana & Prometheus Server Installation

This guidance assumes the usage of Debian as the backend server for handling the monitoring situation, the process *should* be similar across distributions, with exception of the package manager. 

1. Create a fresh installation of Debian, disable *Debian Desktop Environment* and *Gnome*, enable *SSH Server*
2. Install the required dependency packages:

```bash
sudo apt install apt-transport-https wget gpg
```

3. Create a directory for repo encryption keys, and add the Grafana key:

```bash
sudo mkdir -p /etc/apt/keyrings/
```

```bash
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```

4. Add the *stable* releases repository to the Apt package manager:

```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

5. Update your repositories, then install Grafana and Prometheus:

```bash
sudo apt install grafana prometheus
```

6. Now, enable and start both services:

```bash
sudo systemctl enable --now grafana-server
```

```bash
sudo systemctl enable --now prometheus
```

You can verify that this is running by checking `http://<YOUR-IP>:3000`, the default login credentials is admin/admin. Prometheus can be accessed at `http://<YOUR-IP>:9090`.
## Prometheus Server Configuration

Prometheus is a a piece of data scraping software that is used to gather all of the required information from client machines, let's set it up.

1. Prometheus' configuration is handled by a file located at: `/etc/prometheus/prometheus.yml`
2. We will edit this and add our lab machine names to it:
```yaml

```
