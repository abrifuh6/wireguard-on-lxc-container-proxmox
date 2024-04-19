

# Proxmox Container VPN Setup Guide

This guide provides step-by-step instructions on how to configure Wireguard within a Proxmox container. It details editing configuration files, adjusting permissions, and installing Wireguard.

## Prerequisites

- A Proxmox VE installation up and running. For more information, visit the [official Proxmox VE documentation](https://pve.proxmox.com/wiki/OpenVPN_).
- Basic knowledge of Linux commands and container management.

## Configuring the Container for OpenVPN

### On the Proxmox Host

1. **Edit the LXC Container Configuration**
   Open the configuration file of your container with a text editor like nano. Replace "123" with your actual container ID:
   ```
   nano /etc/pve/lxc/123.conf
   ```
   Append the following lines to allow necessary devices and setup mount points:
   ```
   lxc.cgroup2.devices.allow: c 10:200 rwm
   lxc.mount.entry: /dev/net dev/net none bind,create=dir
   ```

2. **Change the Owner of the Device**
   This command also needs to be run on the Proxmox host, targeting the container's device. Ensure you replace "123" with your actual container ID:
   ```
   chown 100000:100000 /dev/net/tun
   ```

3. **Verify the Ownership**
   Confirm the ownership has been updated correctly:
   ```
   ls -l /dev/net/tun
   ```

## Installing Wireguard

### Server Installation

#### On the Proxmox Host

1. **Download and Run the Installation Script**
   Execute this command on the host to download and run the Wireguard installation script:
   ```
   wget https://git.io/wireguard -O wireguard-install.sh && bash wireguard-install.sh
   ```
   This script automates the server setup of Wireguard and can be found in the [GitHub repository](https://github.com/Nyr/wireguard-install).

### Client Installation

- For clients, visit the [Wireguard install page](https://www.wireguard.com/install/) to find specific instructions for various devices. This step should be done on each client device that will connect to the Wireguard server.

## Resources

- **OpenVPN on Proxmox**: Detailed instructions for configuring OpenVPN in a Proxmox environment are available at the [Proxmox OpenVPN Wiki](https://pve.proxmox.com/wiki/OpenVPN_).
- **Wireguard Installation Script**: For the latest script updates and to report issues, visit the [GitHub repository](https://github.com/Nyr/wireguard-install).

