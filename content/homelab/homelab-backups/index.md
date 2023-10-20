+++
title = "NAS Backups and the Dark USB Arts"
subtitle = 'How to do what you should never do, the best way possible.'
date = "2023-10-16"
author = "Ivy Duggan"
draft = false
tags = [
    'linux',
    'storage',
    'data redundancy',
    'backups',
    'usb',
    'nas'
]
+++

# The Backups Speech

Every discussion about file backups generally revolves around the same paradigm:
> You need to have 3 copies of your data: the production data, a local copy, and an off-site backup.

Similarly, when reading about home servers, there are lots of pieces of (good) advice that talk about the importance of redundancy at every level:

[Proxmox Recommended Hardware](https://www.proxmox.com/en/proxmox-virtual-environment/requirements):
> Fast and redundant storage, best results with SSD disks.

[TrueNas Minimum Hardware Requirements](https://www.truenas.com/docs/core/gettingstarted/corehardwareguide/):
> You do not need an SSD boot device, but we discourage using a spinner or a USB stick. We do not recommend installing TrueNAS on a single disk or striped pool unless you have a good reason to do so. You can install and run TrueNAS without any data devices, but we strongly discourage it.

But... my laptop has one drive and I need more NAS storage as cheap as possible. Also, USB drives have changed a lot in recent years with the advent of faster USB data transfer and NVME SSD storage!

## My Setup

As mentioned in my [homelab design post]({{< ref "homelab-design-v1" >}}), my NAS is a late 2011 Macbook Pro, and even with my swapped-in SSD, I have only 2TB of storage on a single drive.

But, thats not the full story. My homelab now has two laptops, configured identically. My previous build, `nessie`, and my "new" laptop server: `maccy`. While `nessie` has a large internal drive now, `maccy` has a 256GB almost-but-not-quite NVME SSD. However, I do have multiple USB SSDs and a penchant to do anything that forum replies tell me I shouldn't.

### Rule #1 of USB Drive NAS's

Don't talk about USB Drive NAS's. Most questions online about "how do I use a USB drive as my NAS?" end in the same answer: **don't even bother**. And for a long time, this was a reasonable (if curt) answer because USB was *dog slow*.

While file sizes are given in gigabytes, file transfer speeds are given in gigabits per second.[^1] An hour of h.264 1080p video @ 24-30fps is ~ 10GB.[^2] My entire document collection is ~100GB. Here is an estimate of file transfer time against all USB versions.[^3] Bolded transfer times are those over 30 minutes, which I consider to be long enough to be notably annoying.

| Version | Speed (Gbps) | Transfer Time 10GB | Transfer Time 100GB |
| ------- | ------------ | ------------------ | ------------------- |
| USB 1.0 | 0.002 | **16 hours** | **168 hours** |
| USB 1.1 | 0.012  | **2 hours** | **21 hours** |
| USB 2.0 | 0.5 | 3 minutes | **0.5 hours** |
| Ethernet* | 1 | 1.5 minutes | 15 minutes |
| USB 3.0 | 5 | 17 seconds | 3 minutes |
| USB 3.1 | 10 | 8 seconds | 1.5 minutes |
| USB 3.2 | 20 | 4 seconds | 44 seconds |
| USB 4/TB3 | 40 | 2 seconds | 20 seconds |
| SATA 3 | 6 | 14 seconds | 2.5 minutes |

*: my wired network is only 1gbps, as 99% of consumers networks still are.

Since an internal SATA 3 drive is around 6gbps, its only between USB 3.1 & 3.2! Now, most servers don't have high speed USB, but plenty of laptops do! My 2015 Macbook Pro (`maccy`) has USB 3.0, at 5gbps.

![image](images/sata-vs-usb3.png "Here's what the final performance looks like at the server (not client download speeds). My external USB NVME adapter is capable of 10gbps, and the drive inside is also >10gbps, so any performance difference is due to USB bottlenecks.")

#### So USB is fast enough for me

The other issues with USB revolve around it's resiliciency. Sys Admins will never recommend using external storage because it is a terrible idea for any business. God forbid a USB drive with the production database fails, or just gets unplugged when an intern gets caught on a cable![^4]

Critical data does not belong on a USB drive, but none of my data is really that critical. All of my development work is already hosted on SVM solutions. My files are really just old schoolwork, pictures, and videos that I would like to keep safe, but don't need 24/7/365 access with hardware redundancy. The best solution for me is just to run a USB drive and also backup remotely.

### Rule #2: Don't Fail to Boot

My homelab posts are the top 10% of the stuff I have tried, and USB drives have been fighting me for a long time. When designing a NAS with USB storage, remember that they **will disconnect**. It's a 'when, not if' situation. Our implementation has to account for this and mitigate the issues that this can cause.

First off, the USB drive cannot be the boot drive. Booting off of USB is useful sometimes, but when you are already playing in the margins of tech, double dipping is a bad idea. If we allow our server to have a USB drive as a storage drive that is mounted later, we can gracefully deal with disconnections.

`fstab`, the file that lists how to mount all drives at boot, has a few options we need to enable. By default, most systems will either halt at boot or wait for a long timeout to pass before booting. If our USB storage is gone, we still want to be able to access the server's web UI![^5]

With the cockpit UI that our system already has, all we really have to do is plug in the drive, format via the UI, and then work on our `fstab` options. There are many cool file systems, but I prefer to keep everything as simple as possible. One partition on the USB drive, `ext4`, no encryption. For nomenclature, I prefer for USB drives to be mounted at `/mnt/<diskname>`.

After basic configuration, our fstab line looks something like this:

`UUID=f94b134d-4e14-4c05-8f30-19aaa477acb4 /mnt/sandisk auto defaults 0 0`

We'll add a `nofail` to this line, making it like this:

`UUID=f94b134d-4e14-4c05-8f30-19aaa477acb4 /mnt/sandisk auto defaults,nofail 0 0`[^6]

This option means that when booting, we will not fail to boot if we are unable to mount our external drive. There are other issues to deal with, but this is the main one that could break our entire deployment.

### Rule #3: Offsite Backups

Another data backup quote I see bounce around often is "local copies are not a replacement for remote backups", which is true, but is focused again at a business environment.

If a production hard drive dies, you need a copy of those files, right now. Servers typically do this via data redundancy over RAID (or other software solutions) with a drive already connected. Switchover is instant, the database never goes down, the business never loses a client due to a service outtage.

Again, at home, I am the client and my server needs are not critical to my ability to put food on the table. For me, a remote backup is fine. The disaster I am trying to prevent is not 'what happens if hardware fails during peak business', but 'my old laptop drive finally died' or 'my apartment flooded'. So, 1 local copy and remote backups are enough for me.

There are a myriad storage providers, and a myriad of backup solutions. Here is what I have found to work for my uses and be cost effective:

[Duplicati](https://www.duplicati.com/) running inside a Portainer template from [this repo](https://github.com/Lissy93/portainer-templates)
    - This docker image then mounts our data directory (`/mnt/sandisk/`), allowing it to access the data directory.

![image](images/duplicati-mounts.png "This is the portainer config for our volumes. The only one I have added is the `sandisk` one.")

[Backblaze B2](https://www.backblaze.com/cloud-storage) for bulk storage. My original pick was AWS, but B2 is easier for deployments that don't utilize the AWS/AIM/CLI ecosystem.[^7]

Configuration tips:

* I prefer to create a separate Application Key for each folder that my NAS uploads to B2. This way, each folder has its own configuration and is slightly more portable. Most of the backup software will upload files to the cloud provider in a binary format (not readable without the backup software itself).

* Make sure each Application Key only has access to a single bucket. Always keep permissions as minimal as possible, its just a good security practice.

* Encrypt your backups. It's built-in to most backup software, so why risk it?

* Check your backups! Backups that do not work are not backups at all. Restore a file, delete a file, modify a file, download a few gigabytes. Test, test, test!

### Rule #4: Fail Gracefully

USB drives are going to fail, so we need to design our system to fail non-destructively. We've already started to account for this with our new mount options above, which allows our system to continue operating if the USB drive is not found during boot, but there are a few other considerations:

1. Nothing Critical
    * Our system is designed with the ability for our USB drive to fail, but our software implementation also depends on us continuing this assumption. Like we already keep our OS on the built-in drive, we should keep all of our programs, software, docker images, etc. on our built-in drive.
    * The USB drive should only store data files & folders.
1. Don't Break Remote
    * If our USB drive is not found, it is possible that some backup software could error out and assume that the files were deleted, thus causing a mess of our backups.
    * Always test your implementation for this oversight. (Even if the backup software handles this situation properly, adding in our docker environment could lead to weird & undocumented behaviour.)
    * I personally tested this on my duplicati/docker instance, and my deployment properly errors and stops the backup process when it cannot mount our data directory.
1. Don't Break Local
    * Our backup system and our software design account for failure, so our last step is to fail gracefully as a NAS. This can be accomplished via symlinks!
    * Instead of sharing `/mnt/sandisk`, which could fail, we can create a folder `/nas`, and then link `/mnt/sandisk` inside of that folder! Then, our failure state will be not having a directory instead of failing to mount anything at all.
    * This also allows us to easily add more USB drives, following a similar pattern.

On our server we simply run:

```bash
sudo mkdir /nas
sudo ln -s /nas/sandisk /mnt/sandisk
```

Then, we can add `/nas` as an nfs or SAMBA share via the Cockpit UI, and any drives that are symlinked under that folder will also be shared.

### Rule #5: Don't talk about USB Drive NAS's

We have mitigated the downsides to USB drives, increased our homelab server storage and resiliency, and have a backup system for when the USB drives eventually break. Plus, we didn't have to buy anything new! Our system fails gracefully in all of the situations covered above, and the following are the steps to fix most problems:

1. Turn off the server.
1. Replug the USB drive.
1. Restart the server.

Yet, the final rule is still the first rule. Being a fault-tolerant, slightly janky system is good for home use, but it is always worth doing it right when actual business is on the line. Please do not apply these to production deployments!

##### Footnotes

[^1]: <https://en.wikipedia.org/wiki/Data-rate_units>
[^2]: <https://www.omnicalculator.com/other/video-size>
[^3]: <https://techinternets.com/copy_calc>
[^4]: If your production deployment relies on external drives to begin with, you may have problems that an internet blog post cannot solve.
[^5]: Since my laptops are also docker hosts, this would also mean all containers would be halted even if they did not interact with the external storage.
[^6]: Make sure to have the entire `defaults,nofail` section without any spaces. The spaces are what designate the columns in the `fstab` file.
[^7]: AWS is very powerful, but with great power, comes great configuration.
