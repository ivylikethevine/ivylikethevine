+++
title = "Laptop NAS - Design"
date = "2023-10-02"
author = "Ivy Duggan"
draft = true
+++


I recently (finally) bought a 3D printer (posts to come), and the main lesson I learned from it was that I did not have a coherent work flow for anything and this was *a problem*.


If you've ever dealt with raspberry pi-based system there is one reality we all have faced: SD card errors. No matter what, an SD card will eventually break (most likely due to user error<sup>Note 1</sup>). With my octoprint setup, reconfiguring everything took a lot of time (and since application keys were regenerated, reconfiguration was also required on the system used to process files before printing). 

Solution -> Octoprint's built-in backups -> Problem 2

-- Working off a server is also really nice -- amazon lesson -- also genetics processing
- add specific lessons from contracting?

default octoprint backups are to local storage -- single zip file

Raspberry pi's are great, but limited. needed low overhead, easily configured way to copy -- built-ins rsync + nfs


### Clients List:
- Expy (Win10/Linux): Up & Down
- Eddie (Linux): Up
- Winnie (Win10/Linux -> Win10): Down
- Steamy (Linux): Down

### Server List:
- Maccy (Linux): Up & Down
- Nessie (Linux): Up & Down

### Design Principles
1. Open - 
1. Adaptable - 
1. Congruency - 

## Constraints
1. Cost
1. Standardized
1. Performance
1. Storage Size
1. Networking
1. Roommate - Can't break DNS/internet


## Laptop Benefits
1. Somewhat power efficient
1. Usually e-waste otherwise
1. Built in battery backup
1. Built in networking (wifi + often ethernet for dual connectivity)
1. Less heat/noise
1. Easier to put somewhere
1. Ability to debug on the device (if its working)
1. Instability

## Jank
- BRIEF digression on linux "you shouldn't mentality" -- re : usb drives
-- user should get exactly what they expect

### Selected Technologies
- Hypervisor: None
  - Alternatives: Proxmox, unraid, freenas, vmware
    - While powerful, additional overhead (both compute & configuration) seemed overkill + configuring boot etc. already had some limits on my macbooks (not UEFI on 2012?)
- OS: Ubuntu 22.04LTS
  - Alternatives: Debian, RHEL, Arch, etc.
    - Ubuntu has modern features, lots of community support. Janky, but WIndows level janky ish.
- Management UI: Cockpit + Portainer
  - Alternatives: Webmin, CasaOS, Yacht
    - Adding additional repos is not great lol
    - CasaOS doesn't have enough power, yacht is janky
- Virtualization: Docker
  - Alternatives: Cockpit-podman, LXC
      - Has drastically less features/less community support, not using a hypervisor in V1
- NAS Protocol: NFS & rsync
  - Alternatives: Samba, Borgmatic
      - Samba is Windows optimized, and Borgmatic is more useful with dedicated backup server, not remote storage.
- Backup Solution: cron/rsync + backup container (kopia/duplicati?) to B2
  - Alternatives: AWS S3/Glacier, other cloud providers
      - Backblaze B2 is good price for archival (downloading only in case of hardware failure)
- Client Connections: Built in NFS mount, SSH keys, and hostname.local domains with mDNS
  - Alternatives: Static IPs (either reserved on DHCP at the router, or with custom DNS server)
    - Laptop can't be relied on for entire network, but static IPs are a pain to change per-client.
