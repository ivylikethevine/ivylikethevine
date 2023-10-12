+++
title = "Laptop NAS - Build v1"
date = "2023-10-02"
author = "Ivy Duggan"
draft = false
+++


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


### Clients List:

- Expy: Up & Down
  - Dell XPS 9300 - Ubuntu 22
- Eddie: Up
  - Raspberry Pi 3 - Octoprint (Rasbian)
- Winnie: Down
  - Gaming PC - Windows 10
- Steamy: Down
  - Steam Deck - SteamOS

### Server List:

- Maccy: Up & Down
  - 2012 Macbook Pro 13" - Ubuntu 22 Server
- Nessie: Up & Down
  - 2015 Macbook Pro 15" - Ubuntu 22 Server
