# Installing-KVM-on-RHEL-9

# KVM Installation and Configuration Guide

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [1. Check Whether Virtualization is Enabled or not](#1-check-whether-virtualization-is-enabled-or-not)
3. [2. Install Virtualization Packages](#2-install-virtualization-packages)
4. [3. Start and Enable Libvirtd Virtualization Daemon](#3-start-and-enable-libvirtd-virtualization-daemon)
5. [4. Configure Network Bridge for KVM](#4-configure-network-bridge-for-kvm)
6. [5. Create Virtual Machine using Virt-Manager GUI](#5-create-virtual-machine-using-virt-manager-gui)

---

## Prerequisites

Before proceeding with KVM installation, ensure the following prerequisites are met:

- Minimal installed RHEL 9 with Desktop Environment
- Sudo user with admin rights
- Local Yum Repository or Red Hat Subscription
- Internet Connectivity (for Red Hat Subscription)

Once the prerequisites are met, proceed with the installation steps of KVM.

---

## 1. Check Whether Virtualization is Enabled or not

To get started, verify if your system supports Virtualization. This is typically enabled in the BIOS by default. To check if Virtualization is enabled on your system, run the following commands:

### For Intel CPUs

```bash
$ grep -e 'vmx' /proc/cpuinfo



If you are running an Intel CPU, and the output confirms that virtualization is already enabled, you are good to go.
For AMD CPUs

$ grep -e 'svm' /proc/cpuinfo

Alternatively, you can run the following command to verify the Virtualization status:

$ lscpu | grep Virtualization

Additionally, check if KVM modules are loaded:

$ lsmod | grep kvm

2. Install Virtualization Packages

Next, we need to install the required virtualization packages. Before installation, it's a good idea to refresh the repositories and install any available updates:
Update the system

$ sudo dnf update -y

Now, install the virt-install and virt-viewer packages:

$ sudo dnf install virt-install virt-viewer -y

    virt-install: A command-line tool for creating virtual machines.
    virt-viewer: A lightweight UI tool that lets you interact with KVM virtual machines using VNC or SPICE remote desktop protocols.

Then, install the libvirt virtualization daemon:

$ sudo dnf install -y libvirt

After installing the libvirt daemon, install virt-manager, a Qt-based graphical interface for managing virtual machines:

$ sudo dnf install virt-manager -y

Also, install additional virtualization tools:

$ sudo dnf install -y virt-top libguestfs-tools

3. Start and Enable Libvirtd Virtualization Daemon

Once the necessary virtualization packages are installed, start and enable the libvirtd daemon using the following commands:

$ sudo systemctl start libvirtd
$ sudo systemctl enable libvirtd

To verify if the daemon is running, use:

$ sudo systemctl status libvirtd

4. Configure Network Bridge for KVM

In this step, you'll configure the network bridge required for KVM. A network bridge is essential for virtual machines to connect to the network. You can use tools like nmcli or network-scripts to create and configure the bridge.

    Note: This part of the guide assumes you have a basic understanding of network configurations in Linux.

Example of creating a bridge using nmcli

$ sudo nmcli connection add type bridge con-name br0 ifname br0
$ sudo nmcli connection add type ethernet con-name br0-port1 ifname eth0 master br0
$ sudo nmcli connection up br0

Replace eth0 with your actual interface name. This configuration allows virtual machines to use the network bridge for accessing the network.
5. Create Virtual Machine using Virt-Manager GUI

Now that the required packages and network configuration are set up, you can create a virtual machine using the Virt-Manager GUI tool.

    Open the GNOME search tool.
    Search for Virtual Machine Manager and launch it.

In Virt-Manager, you can create a new virtual machine by following these steps:

    Click on the File menu and select New Virtual Machine.
    Choose the installation method (e.g., local install media, network install, etc.).
    Follow the on-screen prompts to configure the virtual machine's resources (e.g., memory, CPU, disk size).
    After configuration, click Finish to create and start the VM.

Conclusion

With these steps completed, your KVM installation and configuration should be functional. You can now proceed to create and manage virtual machines using the virt-manager GUI or the command-line tools.


### Changes made:
- I have added **all** the required steps in one complete guide.
- I've included detailed instructions for configuring the network bridge and using `virt-manager` to create a virtual machine.
- I've maintained the clarity and structure, so the guide remains easy to follow.

This version should now have all the information you need, in the correct order, with each section clearly defined.


----
Best Regards
Arjun Kumar
