# Cloudflared - MIPSLE Cloudflare Tunnel Client

Cloudflare Tunnel client for MIPSLE devices.  
Tested on Keenetic Giga (KN-1010).  
Current version: 2025.9.1
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

## Installation

### Precompiled Binary

Compiled binary from [cloudflared](https://github.com/cloudflare/cloudflared) for MIPSLE devices. Tested on Keenetic Giga (KN-1010) with Keenetic OS 4.3.6.1 and Entware.

1. Connect to OPKG SSH:

   ```bash
   opkg install wget-ssl
   # If wget already exists and complains about HTTP/FTP:
   opkg remove wget-nossl
   ```

2. Download the init script:

   ```bash
   wget -O /opt/etc/init.d/S99cloudflared https://raw.githubusercontent.com/furkankadirguzeloglu/cloudflared/main/S99cloudflared
   nano /opt/etc/init.d/S99cloudflared
   # Replace %yourtoken% with your Cloudflare Zero Trust Tunnel token
   chmod +x /opt/etc/init.d/S99cloudflared
   ```

3. Download the Cloudflared binary:

   ```bash
   wget -O /opt/home/cloudflared https://github.com/furkankadirguzeloglu/cloudflared/raw/main/cloudflared
   chmod +x /opt/home/cloudflared
   ```

4. Start the service:

   ```bash
   /opt/etc/init.d/S99cloudflared start
   # Or reboot your router; after ~60 seconds, Cloudflared will be online and visible in the Cloudflare Zero Trust Dashboard
   ```

### Build Yourself

Compiled binary from [cloudflared](https://github.com/cloudflare/cloudflared) for MIPSLE devices. Tested on Keenetic Giga (KN-1010) with Keenetic OS 4.3.6.1 and Entware.

1. If you want to build it yourself, first install Go Lang:

   ```bash
   apt update && apt upgrade
   ```

2. Clone the repository:

   ```bash
   git clone https://github.com/cloudflare/cloudflared
   cd cloudflared
   ```

3. Install the build toolchain:

   ```bash
   ./.teamcity/install-cloudflare-go.sh
   ```

4. Check for success message:
   ```bash
   Installed Go for linux/amd64 in /tmp/go
   Installed commands in /tmp/go/bin - all ok
   ```
   
5. Check for success message:
   ```bash
   Installed Go for linux/amd64 in /tmp/go
   Installed commands in /tmp/go/bin - all ok
   ```

6. Compile Cloudflared for MIPSLE:
   ```bash
   TARGET_ARCH=mipsle GOMIPS=softfloat make cloudflared
   ```

After compiling, the `cloudflared` binary will be available in the folder.

## Usage

Connect Cloudflared to your Cloudflare Zero Trust Tunnel:

```bash
/opt/etc/init.d/S99cloudflared start
```

For more configuration options, visit [Cloudflare Tunnel Docs](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/).

## Author / Acknowledgements

- **Author:** Furkan Kadir Guzeloglu  
- Forked from [DarkAssassinUA/cloudflared](https://raw.githubusercontent.com/DarkAssassinUA/cloudflared)  
- Inspired by the original Cloudflared project: [cloudflared](https://github.com/cloudflare/cloudflared)  
- Thanks to the Cloudflare team for providing a secure tunneling solution.  

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

