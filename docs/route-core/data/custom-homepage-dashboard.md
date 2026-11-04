# Setting up Homepage

This guide will walk you through installing and configuring the **Homepage service** on Fedora Linux. Homepage provides a customisable dashboard where you can monitor useful information such as **weather, system resources, network status, and time/date**.

Official documentation: [gethomepage.dev](https://gethomepage.dev/configs/)

To support with this activity, the below pages may be of use:

* [Linux Package Management](../../misc/linux-package-management.md)
* [The GNU Nano Text Editor](../../misc/the-gnu-nano-text-editor.md)

---

## 1. Install Dependencies

Homepage runs best in Docker.

```bash
# Update Fedora
sudo dnf update -y

# Install Docker
sudo dnf install -y docker docker-compose

# Enable and start Docker
sudo systemctl enable --now docker
```

Verify Docker is running:
```bash
docker --version docker-compose --version
```

---

## 2. Setup Homepage with Docker Compose

Create a homepage folder:

```bash
mkdir ~/homepage && cd ~/homepage
```

Create a docker-compose.yaml file:

```bash
sudo nano docker-compose.yaml
```

Once opened, add this boilerplate code:

```bash
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    volumes:
      - ./config:/app/config
    ports:
      - 3000:3000
    restart: unless-stopped
```

---
## 3. Configure Homepage

All configuration files must be stored in ~/homepage/config/

### Settings files

Create a config folder, then create a settings.yaml file within it:

```bash
mkdir config

nano config/settings.yaml
```

We will add some basic starter code:

```yaml
title: "Fedora Dashboard"
theme: dark
```

Save and Exit by pressing Ctrl + X, confirm save buffer, and file name.

Start the Homepage container:

```bash
sudo docker-compose up -d
```

In your web browser, open http://localhost:3000

---

## 4. Adding Widgets

Remember, after modifying any of the configuration files you must restart the docker container. 

```bash
sudo docker-compose down
sudo docker-compose up -d
```

### Time & Date

Create a new widgets.yaml file:

```bash
nano config/widgets.yaml
```

Inside, place the following code:

```yaml
- datetime:
    format:
      timeStyle: short
      dateStyle: long
```

### System Resources

Add the following to your config/widgets.yaml file:

```yaml
- resources:
    cpu: true
    memory: true
    network: true
```

### Weather 

For a bonus, add weather functionality. For this you will need to create an account on [OpenWeatherMap](https://home.openweathermap.org/users/sign_up) and generate an API key. The add the following to your widgets file:

```yaml
- openweathermap:
    label: "Dover"
    latitude: 51.128
    longitude: 1.311
    units: metric
    provider: openweathermap
    apiKey: "YOUR_API_KEY_HERE"
    cache: 5
```

Save and Exit by pressing Ctrl + X, confirm save buffer, and file name.

---

## 5. Restart Homepage

After editing and saving your yaml files, restart the Homepage service:

```bash
sudo docker-compose restart
```

Check http://localhost:3000 for change

--- 

## 6. Additional Widgets

There are many different widgets that you can implement into Homepage, full list available here:

[Homepage Widgets](https://gethomepage.dev/widgets/)
