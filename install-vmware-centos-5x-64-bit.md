---
copyright:
  years: 2014, 2018
lastupdated: "2018-05-18"
---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Installing VMware on CentOS 5.x (64-bit)

Before you begin, log in to the server with the root user.

## Preparing to install VMware

1. Download the [VMware installer ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://vmware.com/download/server/){: new_window}. Click the download link, accept the EULA, and download the LinuxTarball (VMware-server-1.0.3-44356.tar.gz in this example):

* `# wget –O vmware-server.tar.gz http://download3.vmware.com/software/vmserver/VMware-server-1.0.3-44356.tar.gz`

2. After you download the software, you will need to get a license key (which is free in the free version of VMWare Server).To register, see [VMware ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://register.vmware.com/content/registration.html){: new_window}.

3. Extract the tarball:

* `# tar -xzvf vmware-server.tar.gz`

## Before you begin installation
Before you begin the installation, review the following prerequisites:

```
# yum update
# yum install libXtst.i386
# yum install libXrender.i386
# yum install xinetd
```

## Installing VMWare Server

1. Go to the directory:

* `# cd vmware-server-distrib/`

2. Run the VMware install script:

* `# ./vmware-install.pl`

3. The installation is going to ask you some basic questions and wants to know what directories to create and install certain parts of VMware into. From here, accept the defaults. Accept the license agreement to proceed with the installation.

4. You might be asked this question: “None of the pre-built vmmon modules for VMware Server is suitable for your running kernel. Do you want this program to try to build the vmmon module for your system (you need to have a C compiler installed on your system)?”
* Answer **yes** to this question (which is the default).

## Setting up VMware networking
You will be prompted with the following questions:

1. "Do you want networking for your virtual machines?" 
* Answer **yes**. You need to create a network setup for your public network device so that you can access the internet on your virtual machines.

2. “Your computer has multiple ethernet network interfaces available: eth0, eth1. Which one do you want to bridge to vmnet0?”
* This is a very important question. Remember, the way all {{site.data.keyword.BluSoftlayer}} servers are set up and run are with the public network running on eth1 and the private network running on eth0. In VMware, the default bridge device for vmnet0 is eth0. Instead of pressing enter, type in eth1.

## Bridging the private network 
You will then be prompted with the following question, which can be answered as **yes** or **no**:
*“Do you want to configure another bridged network?”

If you plan on running services or other applications off of your private network, then proceed with “yes” to this question (unless you know you will not be using the private network), and a network bridge is create to your private network. After you press enter, it will automatically use eth0 as the interface, as that is the only one left available (since you only have two network cards in the server).

## Other Networking Settings

You will be presented with a few other questions regarding the network setup of VMware Server. Proceed with the following recommendations:

* *“Do you want to be able to use NAT networking in your virtual machines?”*

- Proceed with “yes”

* *“Do you want this program to probe for an unused private subnet?”*

- Proceed with “yes”

- After this completes, make sure you do not configure another NAT network.

* *“Do you want to be able to use host-only networking in your virtual machines?”*

- Proceed with “yes”

* *“Do you want this program to probe for an unused private subnet?”*

- Proceed with “yes”

- Once this completes, make sure you do not configure another host-only network.

## Specifying listening port

The next question it is going to ask is what port you are wanting VMware Server to listen on. You can leave the default port as 904.

## Storing the virtual machines

The next question that the installer asks is *“In which directory do you want to keep your virtual machine files?”*. The default place is /var/lib/vmware/Virtual Machines. Make sure to place the virtual machines in a place where you have plenty of disk space, such as a redundant RAID array or a large secondary hard drive. Always make sure that you have enough room for a virtual machine. In this case, you could use a mount point /data/vm that is mounted to a large disk.

## Providing the serial number for VMware

The final part of the installation requires you to insert a VMware license key and serial number. If you have not generated a license key yet, see the [VMware site ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://register.vmware.com/content/registration.html){: new_window}. If you have the serial number for this VMware Server, insert it into the prompt and then press enter.

You should see something similar to the following:

    “The configuration of VMware Server 1.0.3 build-44356 for Linux for this running kernel completed successfully.”

VMware is now set up on your server. You now need to download the VMware Server Console, which is the GUI client for your VMware Server that allows you to set up, configure, and install virtual machines.

## Downloading VMware Server Console

The VMware Server Console is the client application for VMware Server. It allows you to manage the VMware Server in which you can create, configure, and install virtual machines. To install this application, download the VMware Server Windows client package from [VMware downloads ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://vmware.com/download/server/){: new_window}. This package is the .zip file. After it downloads, extract the package and install the VMware-console-1.0.3-x file. When this has completed installing, you are done installing the VMware Server Console and you are ready to configure your VMware Server.

## Logging into the VMware Console

Open the VMware Server Console from the computer you installed it on. When it loads you will be prompted with a “Switch Host” (login) screen. VMware Server uses the Linux system username/passwords to authenticate users, so you will need to use the usernames (root in particular) to login to VMware. Use the following credentials:

* **Hostname:** IP address plus port (e.g. 67.228.160.201:904)<br />
* **User Name:** root<br />
* **Password:** password (use the real root password of the system)

## Configuring the firewall rules (IPTables)

If you have issues connecting to the VMware server and it is not an authentication issue (if you get a username/password error then you have a bad user or password), then your firewall might be blocking you from connecting to the VMware Server. To resolve this, try adding the following IPTable rule into your /etc/sysconfig/iptablesfile (and make sure that the naming convention follows your server configuation):

- `# -A FWALL-INPUT -p tcp -m tcp -s 0/0 --dport 904 -j ACCEPT`


