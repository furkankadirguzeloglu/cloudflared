# Cloudflared - MIPSLE Cloudflare Tunnel Client

Cloudflare Tunnel client for MIPSLE devices.  
Tested on Keenetic Giga (KN-1010).  
Current version: 2026.1.2
Built with Go 1.25.1

## Purpose

Cloudflared provides secure tunneling with Cloudflare Zero Trust.  
This project offers a compiled client to run Cloudflared on MIPSLE-based devices.
**Note:** This project is forked from [DarkAssassinUA/cloudflared](https://raw.githubusercontent.com/DarkAssassinUA/cloudflared) and adapted for MIPSLE devices.

## Requirements

- MIPSLE-based device (e.g., Keenetic Giga)
- Cloudflare account with Zero Trust configuration
- Internet connection

## Warning

All software provided here is provided **as-is** without any questions or complaints.  
Please direct all questions and complaints to the original [cloudflared](https://github.com/cloudflare/cloudflared) repository.  

This is just an assembly for MIPSLE devices, compiled from the source code **without any changes**.  
All open issues related to the work of Cloudflared itself will be closed without response. You have been warned.

## Installation Guide

### Precompiled Binary

Instructions for installing the precompiled Cloudflared binary on **MIPSLE devices**.
Tested on **Keenetic Giga (KN-1010)** running **KeeneticOS 4.3.6.1** with **Entware**.

#### Step 1: Install Required Packages

Install essential utilities:

```bash
opkg update
opkg install nano
opkg install cron
```

- `nano` is a text editor for editing configuration files.
- `cron` is required to schedule automatic service startup.

#### Step 2: Download and Configure the Init Script

Download the initialization script to manage the Cloudflared service:

```bash
wget --no-check-certificate -O /opt/etc/init.d/S99cloudflared https://raw.githubusercontent.com/furkankadirguzeloglu/cloudflared/main/S99cloudflared
nano /opt/etc/init.d/S99cloudflared   # Replace %yourtoken% with your Cloudflare Zero Trust Tunnel token
chmod +x /opt/etc/init.d/S99cloudflared
```

- Insert your **Cloudflare Zero Trust Tunnel token** into the script.
- Make the script executable for proper service management.

#### Step 3: Download the Cloudflared Binary

```bash
wget --no-check-certificate -O /opt/home/cloudflared https://github.com/furkankadirguzeloglu/cloudflared/raw/main/cloudflared
chmod +x /opt/home/cloudflared
```

- The binary is tailored for MIPSLE devices and compatible with Keenetic routers.

#### Step 4: Start the Cloudflared Service

```bash
/opt/etc/init.d/S99cloudflared start
# Or reboot the router. Cloudflared starts automatically within ~60 seconds.
```

- Verify the service status in the **Cloudflare Zero Trust Dashboard**.

#### Step 5: Enable Automatic Startup on Reboot

```bash
export VISUAL=nano
export EDITOR=nano
crontab -e
# Add the line:
# @reboot /opt/etc/init.d/S99cloudflared start
reboot
```

- This sets up a cron job to launch Cloudflared at system startup.
- Cloudflared will run automatically after reboot.

---

### Build Cloudflared from Source

Instructions for compiling Cloudflared for **MIPSLE devices**. Tested on **Keenetic Giga (KN-1010)** with **KeeneticOS 4.3.6.1** and **Entware**.

#### Step 1: Install Go Language

Ensure Go is installed on your system:

```bash
apt update && apt upgrade
```

Follow system-specific instructions if Go is not available.

#### Step 2: Clone the Cloudflared Repository

```bash
git clone https://github.com/cloudflare/cloudflared
cd cloudflared
```

#### Step 3: Install the Build Toolchain

```bash
./.teamcity/install-cloudflare-go.sh
```

Expected confirmation:

```bash
Installed Go for linux/amd64 in /tmp/go
Installed commands in /tmp/go/bin - all ok
```

#### Step 4: Compile Cloudflared for MIPSLE

```bash
TARGET_ARCH=mipsle GOMIPS=softfloat make cloudflared
```

- The `cloudflared` binary will appear in the current directory after successful compilation.

This ensures you maintain the latest Cloudflared version optimized for your MIPSLE device.

## Usage

Connect Cloudflared to your Cloudflare Zero Trust Tunnel:

```bash
/opt/etc/init.d/S99cloudflared start
```

For more configuration options, see the Cloudflare Tunnel Docs: https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/

## Author / Acknowledgements

- **Author:** Furkan Kadir Guzeloglu
- Forked from DarkAssassinUA/cloudflared
- Inspired by the original Cloudflared project: https://github.com/cloudflare/cloudflared
- Thanks to the Cloudflare team for providing a secure tunneling solution.

## License

This project is licensed under the MIT License: https://opensource.org/licenses/MIT
