+++
title = 'Laptop Homelab in 5 Commands'
date = '2023-10-12'
subtitle = 'A quick & simple NAS/server from my old laptop.'
author = 'Ivy Duggan'
draft = false
tags = [
    'linux',
    '3d-printing',
    'DNS',
    'networking',
    'docker',
    'homelab',
    'nas'
]
header_img = ''
description = ''
toc = true
categories = []
series = []
+++
<a href='https://www.reviewjournal.com/business/no-lazy-kid-12-year-old-app-developer-off-to-fast-start'>

  ![image](images/macbook-panera.webp "Back when I developed on my Macbook! ")
  
</a>

# The Background

I have been in tech for over 10 years (look at little me up there), and I have seen firsthand that a proper workflow is essential for any decent project. So, when I bought a 3D printer a few months ago, it was embarrassing to see **my** lack of workflow. Dealing with the dozens of STL files was a hassle by itself, but then, it came: *Raspberry Pi SD card corruption.* And it came again... and again... and again...

With least an hour of configuration per corruption, I finally bit the bullet and started setting up my own server. **tl;dr? here's the [full setup script](https://gist.github.com/ivylikethevine/adba9472047741476e5a19b8741e906a)**

## Why a Home Server?

I have avoided making my own server for the past few years because I did not have a need for one. At Amazon Music, we had access to the internal architecture & work flows, and during my contracting work, our applications were usually relatively straightforward GIT/CI/CD Node/similar applications.[^1] But now, I have graduated college and have the time & skill to setup my own, so lets do it --- on the cheap.

### Enter... the Laptop Server

No, a laptop is not the ideal server. They lack multiple ethernet connections, extra storage, and were not designed for this use-case. However, for personal-use deployment, they come with many benefits:

1. Power Efficiency
   - Laptops in general are built for battery life, and thus are designed to draw less power than a desktop/server often would.
   - Less power use = less waste heat. This is critical to me since the server must be in my bedroom to have access to ethernet.
1. It's a Laptop
   - Small, easy to place wherever. (Mine sits under my TV)
   - Instead of a UPS, the laptop battery itself provides a healthy backup when power is cut-out (or more likely, when there are power fluctuations).
   - Built in keyboard/monitor for debugging **when** something breaks.[^2]
1. I already have one. While I would love a full custom rack setup, **the best server is the server you have *right now***.

### The Hardware - Nessie, the 2012 Macbook Pro

For my setup, my server hardware is my old 2012 Macbook Pro 13":

- [Intel Core i5-2430M](https://www.intel.com/content/www/us/en/products/sku/53450/intel-core-i52430m-processor-3m-cache-up-to-3-00-ghz/specifications.html): 2 cores + hyperthreading, 2.4-3.0Ghz
- 2 x 2GB DDR3 1333MT/s RAM
- 1x Gigabit Ethernet
- 1x Thunderbolt 2 Port
- 2TB Crucial SATA SSD - Upgraded from original 128GB SSD.

The specs are not incredible, and I was uncertain if this would be a performant system (spoiler: it works fine!). I upgraded the SSD with one I had laying around, since 128GB was not sufficient. Production servers should **always have redundant storage and networking**, but beggers can't be choosers! It is possible to adapt the Thunderbolt 2 port to Gigabit ethernet, but dual networking is a rabbithole to follow another day.

### The Software - Keep it Simple

- OS: [Ubuntu 22.04LTS](https://ubuntu.com/download/server)
- Host Management: [Cockpit](https://cockpit-project.org/)
- Container Management: [Portainer-CE](https://docs.portainer.io/start/install-ce)
- Virtualization: [Docker](https://www.docker.com/)
- NAS Mount Protocol: [NFS](https://en.wikipedia.org/wiki/Network_File_System)

There will be a future post going more in-depth about the specific tech selection! Many popular alternatives were tested or considered, but I arrived at this setup because each of these technologies are well known, well developed, and have tons of community support & documentation.

# Initial Installation

My servers all run Ubuntu 22.04LTS.[^3] Installation of Ubuntu is relatively easy, and the only install configuration required is "Install OpenSSH Server". Aside of that, the default settings work fine. [Installation Instructions](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview).

Note: this guide assumes that the laptop is plugged into ethernet during the Ubuntu installation process.

After installing and rebooting, we need to `ssh` into the laptop to configure it. At this point, our system has a Dynamically Assigned IP address (such as 192.168.1.10), which we need to know in order to connect. If you have access to your router login, simply find the IP address assigned to the `hostname` of the server, in my case `nessie`. If you don't have access to that, then login to the server on the laptop (w/ the username & password from installation), and run the following command: `hostname -I | awk '{print $1}'`. The result is the current IP of the host. The next commands can be run on the device itself, but I find it easier to connect to the host & then copy paste from my normal laptop (versus trying to manipulate text from a command line). So let's configure us a server!

## The Five Command(ments)

### I. Thou art a Server

Although we're installing a server OS, there are a few things we want to configure to allow our laptop to "act like a server." This command does 2 things:

1. Prevent system from sleeping when the lid is closed.
1. Turn off the screen after 60 seconds (but not fully disable -- to allow on-device debugging).

{{< highlight bash >}}
sudo echo $'HandleLidSwitch=ignore\nHandleLidSwitchDocked=ignore' | sudo tee -a /etc/systemd/logind.conf &&
  sudo echo "GRUB_CMDLINE_LINUX_DEFAULT=\"consoleblank=60\"" | sudo tee -a /etc/default/grub &&
  sudo update-grub
{{< / highlight >}}
** The `&&` chaining syntax means that the next command will not execute until/if the previous command executes successfully.

### II. Thou Shalt Have Basics

Ubuntu 22.04LTS comes with many useful packages, but there are a few that I always end up using, so I've put them here. Of note:

1. The `$nrconf` line prevents a pop-up window listing services that should be restarted. This halts our config, so I have altered the configuration to list and not pop-up and halt.
1. `avahi-daemon` and `avahi-utils` are critical for our link-local addressing.
1. `nfs-common` is the package we will use to access the files on the NAS as if it were an external drive plugged.

{{< highlight bash >}}
sudo echo "\$nrconf{restart} = 'l'" | sudo tee -a /etc/needrestart/needrestart.conf &&
  sudo apt update -y &&
  sudo apt upgrade -y &&
  sudo apt install -fy zip git htop software-properties-common apt-transport-https wget xclip net-tools curl python3 python3-pip nodejs npm ca-certificates gnupg avahi-daemon avahi-utils nfs-common &&
  sudo apt autoremove -y
{{< / highlight >}}

### III. Thou Shall have Containers

In the Homelab/Selfhosted/NAS space, containerized applications are common (and awesome!). Docker lets our server run containers, meaning we can run software on our host server without having to configure the host for every single application. Each container is separate and (more or less) sandboxed.[^4] In the case of deploying templates, containers are essentially "applications" that run on our server.

{{< highlight bash >}}
sudo install -m 0755 -d /etc/apt/keyrings &&
  curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg &&
  sudo chmod a+r /etc/apt/keyrings/docker.gpg &&
  echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" |
  sudo tee /etc/apt/sources.list.d/docker.list >/dev/null &&
  sudo apt update -y &&
  sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin &&
  sudo usermod -aG docker $USER &&
  newgrp docker &&
  sudo systemctl enable docker
{{< / highlight >}}

### IV. Thou Shalt be Web Monitored

For this server, two different monitoring solutions are needed.

1. `cockpit` - managing the host device & NAS file sharing
    - we also install `cockpit-file-sharing` from 45Drives, which allows easy NAS setup from the cockpit webUI
1. `portainer` - a containerized application that allows the monitoring & deployment of other containerized applications

{{< highlight bash >}}
sudo mkdir /portainer_data &&
  docker volume create portainer_data &&
  docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/portainer_data portainer/portainer-ce:latest &&
  sudo apt install -y cockpit cockpit-pcp &&
  curl -LO <https://github.com/45Drives/cockpit-file-sharing/releases/download/v3.2.9/cockpit-file-sharing_3.2.9-2focal_all.deb> &&
  sudo apt install -y ./cockpit-file-sharing_3.2.9-2focal_all.deb &&
  rm ./cockpit-file-sharing_3.2.9-2focal_all.deb
{{< / highlight >}}

### V. Thou Shalt Not Break DNS

After hours of blood, sweat, and tears, I've arrived at a networking configuration that satisfies the following:

1. Work with `cockpit`. (Requires use of `NetworkManager` for proper updates & network monitoring in the Web UI, not `networkd`)
1. Don't break DNS. (Can't run my own DNS server/sinkhole)
1. Use a named address & a dynamic IP. (`<hostname>.local` with DHCP)

{{< highlight bash >}}
sudo echo "network:
  version: 2
  renderer: NetworkManager
  ethernets:
    $(ip -o -4 route show to default | awk '{print $5}'):
      dhcp4: true
      link-local: [ ipv4 ]" | sudo tee /etc/netplan/00-installer-config.yaml &&
  sudo netplan apply
{{< / highlight >}}

Essentially what I am doing here is overwriting the default wired `netplan` with my own configuration. The renderer is set to `NetworkManager` to work with `cockpit`'s UI. The line `$(ip -o -4 route show to default | awk '{print $5}'):` is a nifty way to grab the current active ethernet connection and configure it (versus unconfigured or virtual interfaces). On my macbook, it evalutes to `ens9`, the name of the built-in ethernet interface. We enable `dhcp4`, since my home network only uses `ipv4`, not `ipv6`, and my router uses DHCP instead of static IPs (as do most home networks).[^5] [^6] [^7]

The `link-local: [ ipv4 ]` line is also crucial, as it works with `avahi-daemon`/`avahi-utils` to tell other devices on our network that `nessie.local` can be used, not a dynamically changing IP address. Alternate solutions involve editing files on every host (which is a hassle), or creating a custom DNS server (which is also a hassle and I could break the wifi for my roommate, which would be very bad!)

After the new `netplan` configuration is applied, our `ssh` session break or disconnect, so close the terminal and wait a few minutes. Then, reboot the machine (from command line: `sudo reboot`). When it finishes booting (which may take a few minutes, internet interfaces can take a while to start), you should be able to access it via `https://<hostname>.local:9090` for cockpit `https://<hostname>.local:9443` for portainer. At this point, all management can be done via cockpit's terminal or portainer's UI!

### Congratulations 🎉

Now just configure the software we've installed & enjoy:

- [Initial Portainer Setup](https://docs.portainer.io/start/install-ce/server/setup)
- Optional: Change portainer templates to 3rd party repo to allow more 1-click application deployments, such as <https://github.com/lissy93/portainer-templates>
- [Configure SAMBA on the server for Windows Clients](https://github.com/45Drives/cockpit-file-sharing#samba-management-tab)
- [Configure NFS on the server for linux clients](https://github.com/45Drives/cockpit-file-sharing#nfs-management-tab)
- Optional: Mount folder at boot on Linux client with the following command (requires `sudo apt install -y nfs-common` first):
{{< highlight bash >}}
sudo echo '<hostname>.local:/<nfs_folder>    /<local_mount>   nfs rw,auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0' | sudo tee -a /etc/fstab &&
  sudo mount -a
{{< / highlight >}}

  - `<hostname>` - ex: nessie
  - `<nfs_folder>` - the name of the nfs folder created above
  - `<local_mount>` - the folder on the client device to alias to the server
    - often the `/media/<folder>` or `/mnt/<folder>`. ex: `/mnt/nessie`
    - folder must be created before the `fstab` file modification[^8], ex: `sudo mkdir /mnt/nessie`

**Next up, we'll configure offsite backups via a containerized application since our server has ZERO redundancy!**

#### Footnotes

[^1]: I also had an experience with a developer above me that "pushed changes to his local server, but forgot to push the changes to us" around every week or so.
[^2]: I **often** broke my ability to communicate with the server during network configuration and had to run commands on the laptop itself to fix it.
[^3]: LTS designation means that this version of the OS will receive support for 5 years. I prefer to run an LTS version of an OS, since I'd rather have stability versus new features (and doubly so on a server).
[^4]: Containerization is more complex than this, and the security implications of containers is its own discussion, but for most users, the simple understanding works.
[^5]: `ipv4` addresses follow a period deliminated format, such as `192.168.1.1` (typically the router's IP address), but `ipv6` uses colon deliminated addresses, such as `fd12:3456:789a:1::1`. `ipv6` is useful in larger deployments, but unnecessary for most home use.
[^6]: In a DHCP network, typically the router assigns IP addresses to all devices, but these addresses (192.168.1.XX) can change when a device is disconnected/restarted. Static IPs do not change, but usually require more configuration.
[^7]: [Netplan Documentation](https://netplan.readthedocs.io/en/stable/netplan-yaml/)
[^8]: `fstab` is a file that essentially lists all drives and how to connect to them on boot.
