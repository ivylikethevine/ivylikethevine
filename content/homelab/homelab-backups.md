+++
title = "NAS Backups and the Dark USB Arts"
subtitle = 'How to do what you should never do, the best way possible.'
date = "2023-10-16"
author = "Ivy Duggan"
draft = true
tags = [
    'linux',
    'storage',
    'data redundancy',
    'server',
    'homelab',
    'nas'
]
+++

# The Backups Speech

Every discussion about file backups generally revolves around the same speech:
> You need to have 3 copies of your data: the production data, a local copy, and an off-site backup.

Similarly, when reading about home servers, there are lots of pieces of (good) advice that talk about the importance of redundancy at every level:

[Proxmox Recommended Hardware](https://www.proxmox.com/en/proxmox-virtual-environment/requirements):
> Fast and redundant storage, best results with SSD disks.

[TrueNas Minimum Hardware Requirements](https://www.truenas.com/docs/core/gettingstarted/corehardwareguide/):
> You do not need an SSD boot device, but we discourage using a spinner or a USB stick. We do not recommend installing TrueNAS on a single disk or striped pool unless you have a good reason to do so. You can install and run TrueNAS without any data devices, but we strongly discourage it.

But... my laptop has one drive and I need more NAS storage as cheap as possible. Also, USB drives have changed a lot in recent years with the advent of faster USB data transfer and NVME SSD storage!

## My Setup

As mentioned in my [homelab design post]({{< ref "homelab/homelab-design-v1.md" >}}), my NAS is a Late 2011 Macbook Pro, and even with my swapped-in SSD, I have only 2TB of storage on a single drive.

But, thats not the full story. My homelab now has two laptops, configured identially. My previous build, `nessie`, and my "new" laptop server: `maccy`. While `nessie` has a large internal drive now, `maccy` has a 256GB almost-but-not-quite NVME SSD. However, I do have multiple USB SSDs and a penchant to do anything that forum replies tell me I shouldn't.

### Rule #1 of USB Drive NAS's

Don't talk about USB Drive NAS's. Most questions online about "how do I use a USB drive as my NAS?" end in the same answer: **don't even bother**. And for a long time, this was a reasonable (if curt) answer because USB was *dog slow*.

An hour of h.264 1080p video @ 24-30fps is ~ 10Gb.[^1]

USB 2.0 speed is 480 megabits/second. = transfer time of ~ 3 minutes

USB 3.0 5Gbps = transfer time of ~ 17 seconds

USB 3.0 10Gbps = transfer time of ~ 8 seconds[^2]

For comparison, an internal SATA 3 drive is around 6Gbps, or between USB 3.1 & 3.2! Now, most servers don't have high speed USB, but my laptop does! And even if I wanted to make use of fast new NVME ssd's, my network is only 1GbE, as I imagine 99% of consumers networks are as well. My wireless network theoretically could acheive multi gigabit with wifi-6, but running file transfer over wifi gives me the heebijeebies.

#### So USB is fast enough for me

The other issues with USB revolve around it's resiliciency. Sys Admins will never recommend using external storage because it is a terrible idea for any business. God forbid a USB drive with the production database[^3] fails, or just gets unplugged when an intern gets caught on a cable!

Critical data does not belong on a USB drive, but none of my data is really that critical. All of my development work is already hosted on SVM solutions. My files are really just old schoolwork, pictures, and videos that I would like to keep safe, but don't need 24/7/365 access with hardware redundancy. The best solution for me is just to run a USB drive and also backup remotely.

##### Local copies versus Remote backups

Another data backup quote I see bounce around often is "local copies are not a replacement for remote backups", which is true, but is focused again at a business environment. If a production hard drive dies, you need a copy of those files, right now. Servers typically do this via data redundancy over RAID (or other software solutions) with a drive already connect4ed. Switchover is instant, the database never goes down, the business never loses a client due to a service outtage.

Again, at home, I am the client and my server needs are not critical to my ability to put food on the table. For me, a remote backup is fine. The disaster I am trying to prevent is not 'what happens if hardware fails during peak business', but 'my old laptop drive finally died' or 'my apartment flooded'.

### Rule #2: Don't Boot

My homelab posts are the top 10% of the stuff I have tried, and USB drives have been fighting me for a long time. When designing a NAS with USB storage, remember that they **will disconnect**. It's a 'when, not if' situation. Our implementation has to account for this and mitigate the issues that this can cause.

First off, the USB drive cannot be the boot drive. Booting off of USB is useful sometimes, but when you are already playing in the margins of tech, double dipping is a bad idea. If we allow our server to have a USB drive as a storage drive that is mounted later, we can gracefully deal with disconnections.

`fstab`, the file that lists how to mount all drives at boot, has a few options we need to enable. By default, most systems will either halt at boot or wait for a long timeout to pass before booting. If our USB storage is gone, we still want to be able to access the server's web UI![^4]

With the cockpit UI that our system already has, all we really have to do is plug in the drive, format via the UI, and then work on our `fstab` options. There are many cool file systems, but I prefer to keep everything as simple as possible. One partition on the USB drive, `ext4`, no encryption. For nomenclature, I prefer for USB drives to be mounted at `/mnt/<diskname>`.

After basic configuration, our fstab line looks something like this:

`UUID=f94b134d-4e14-4c05-8f30-19aaa477acb4 /mnt/sandisk auto defaults 0 0`

We'll add a `nofail` and `nobootwait` to this line, making it like this:

`UUID=f94b134d-4e14-4c05-8f30-19aaa477acb4 /mnt/sandisk auto defaults,nobootwait,nofail 0 0`[^5]

### Rule #3: Offsite Backups

There are a myriad storage providers, and a myriad of backup solutions. Here is what I have found to work for my uses and be cost effective:

[Duplicati](https://www.duplicati.com/) running inside a Portainer template from [this repo](https://github.com/Lissy93/portainer-templates)

[Backblaze B2](https://www.backblaze.com/cloud-storage) for bulk storage. My original pick was AWS, but B2 is easier for deployments that don't utilize the AWS/AIM/CLI ecosystem.[^6]

Configuration tips:

- I prefer to create a separate Application Key for each folder that my NAS uploads to B2. This way, each folder has its own configuration and is slightly more portable. Most of the backup software will upload files to the cloud provider in a binary format (not readable without the backup software itself).

- Make sure each Application Key only has access to a single bucket. Always keep permissions as minimal as possible, its just a good security practice.

- Check your backups! Backups that do not work are not backups at all. Restore a file, delete a file, modify a file, download a few gigabytes. Test, test, test!

### Rule #4: This is not Prod

Please do not apply these to production deployments. Do not suggest these to your company if you enjoy being employed. Heed my warning or suffer.

### Rule #5: Don't talk about USB Drive NAS's

The final rule is the first rule. We have mitigated the downsides to USB drives, increased our homelab server, and have a backup system for when the USB drives eventually break. And hey, we didn't have to buy anything new & we're re-using old technology!

##### Footnotes

[^1]: <https://www.omnicalculator.com/other/video-size>
[^2]: <https://techinternets.com/copy_calc>
[^3]: If your production deployment relies on external drives to begin with, you may have problems that an internet blog post cannot solve.
[^4]: Since my laptops are also docker hosts, this would also mean all containers would be halted even if they did not interact with the external storage.
[^5]: Make sure to have the entire `defaults,nobootwait,nofail` section without any spaces. The spaces are what designate the columns in the `fstab` file.
[^6]: AWS is very powerful, but with great power, comes great configuration.
