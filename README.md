# CentOS 8 Setup Guide
These are my step-by-step instructions for installing CentOS 8.

If you want them to do something else, the [fork button](https://github.com/BlacksilverConsulting/OS8) is right there.

## Hyper-V Note

VM Requirements:
- Generation 2 VM
- Disable SecureBoot

If this VM will be inheriting disks from another (e.g., Scientific Linux 6), then it will usually only need one small (20GB) disk of its own.

## OS Installation

Download the [CentOS 8 stream ISO](https://mirrors.ocf.berkeley.edu/centos/8-stream/BaseOS/x86_64/os/images/install.img).

Boot the VM from that .iso, and the install boot menu will appear.

![Image](/images/01-InstallerBoot.png)

Either press the Enter key or wait for the menu to count down. The installer will verify that the install image was not corrupted during download, then prompt you to choose the language to use during the install.

![Image](/images/02-InstallLanguage.png)

If you prefer a language other than U.S. English, choose it now. (Note that these instructions are only provided in US English.)

Click the Continue button in the lower-right corner to go to the installation summary.

![Image](/images/03-InstallMenu1.png)

We will only be making changes in four of the eleven parts of the installation summary. Start by clicking on **Root Password** to display the root password screen.

![Image](/images/04-RootPassword1.png)

Enter the same strong password in both fields. Then click the Done button in the upper-left-corner to return to the installation summary.

![Image](/images/06-InstallMenu2.png)

Click **Software Selection** to display the software selection screen.

![Image](/images/07-InstallType1.png)

In the left column, click the circle next to **Minimal Install**.

![Image](/images/08-InstallType2.png)

Click the Done button in the upper-left-corner to return to the installation summary, then click **Installation Destination** to display the Installation Destination screen.

![Image](/images/10-Destination.png)

Simply displaying this screen is enough to load the default destination configuration. Click the Done button in the upper-left-corner to return to the installation summary.

![Image](/images/11-InstallMenu4.png)

Click **Network & Host Name** to display the network & host name screen.

![Image](/images/12-Network1.png)

First, enter the new hostname in the field in the lower-left corner, then click the Apply button.

The network configuration will use a DHCP server if one is available, as shown here.

![Image](/images/13-Network2.png)

If the network configuration is acceptable, skip ahead to _Done with Network & Host Name_.

If you do not have a DHCP server, or prefer a different method of configuring the network adapter, click the Configure button in the lower-right corner to edit the configuration.

![Image](/images/14-NetworkAdapter1.png)

Click **IPv4 Settings** to open that tab and change how the network adapter is addressed.

![Image](/images/15-NetworkAdapter2.png)

Click the Save button in the lower-left corner to return to the Network & Host Name screen.

_Done with Network & Host Name_. Click the Done button in the upper-left corner to return to the installation summary.

![Image](/images/17-InstallMenu5.png)

Click the Begin Installation button in the lower-right corner of the installation summary, and the installation will begin. Wait while the installer prepares the disk.

![Image](/images/19-InstallProgress2.png)

Wait while the installer copies the OS to the disk.

![Image](/images/20-InstallProgress3.png)

Wait while the installer configures the OS.

When the installation is complete, the Reboot System button in the lower-right will be enabled.

![Image](/images/21-InstallComplete.png)

Click it and the install media will be ejected as the VM reboots. Wait while the OS starts. 

The VM will display some startup information and end with a mostly-blank screen and a login prompt in the upper-left corner.

![Image](/images/22-OSLoginPrompt.png)

## Ansible Bootstrap

After the OS is installed, there are a lot of setup steps. This project includes some scripts to automate setting up the server in a particular way.

Sign in to the VM as root and run this command:

```bash
cd && curl -LJO https://blacksilverconsulting.github.io/OS8/start.sh && bash start.sh
```

This shell script does this:
- Enable the EPEL repository
- Install Ansible and required dependencies
- Download the base, PostgreSQL, and dm playbooks
- Run the base playbook

The shell script ends with instructions for running two additional Ansible playbooks.

## (Optional) PostgreSQL 14 Server and Client

Use this playbook to install PostgreSQL 14 Server and Client from [The PostgreSQL official site](https://www.postgresql.org/) with some opinionated defaults.

```bash
ansible-playbook ./pg14.yaml
```

## (Optional) Document Management Support
 
Linux-based document management systems benefit from using this playbook to pre-install many useful components.

```bash
ansible-playbook ./dm.yaml
```

## Next steps

After that, things get interesting. We can start attaching disks from other VMs, configuring applications, and getting ready to test.
