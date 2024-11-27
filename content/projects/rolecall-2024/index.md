+++
title = 'Homelab Hardware 2024'
slug = 'Homelab Hardware 2024'
date = '2024-11-27'
subtitle = 'The janky hardware I use, now updated!'
description = 'The janky hardware I use, now updated!'
draft = true
toc = true
tags = [
    'dev ops',
    'homelab',
    'linux',
    'nas',
    'networking',
    'saffron',
]
categories = ['saffron']
+++

# Rolecall

Part of the modus operandi of my work in the homelab space is to re-use old hardware, because:

1. It's a fun challenge to work with sub-optimal hardware.
1. I'm cheap.

As such, here's the current information on my homelab hardware, none of which is very specialized or powerful.

## Lenny, my Old Gaming PC

Role: Experimental Promox & [Saffron](https://github.com/ivylikethevine/saffron) Host

- [PcPartPicker](https://pcpartpicker.com/list/)
- [Ryzen 5 3600x]()
- BRAND 2 x 8GB DDR4 XXXXMT/s RAM
- Asrock Phantom Gaming X570 ITX/TB3
  - 1x Intel 1GBe
- EVGA 750W PSU \*
- Corsair Hyper 212 EVO
- NZXT H210i
- 1TB WD Blue 660p M.2 SATA \*\*
- Geekbench 6 Scores: ~ Single Core/~ Multi Core

## Swervy, the Node 202 Build

Role: Stable Proxmox & [Saffron](https://github.com/ivylikethevine/saffron) Host & NAS

- [PcPartPicker](https://pcpartpicker.com/list/kF8HQP)
- [Intel Core i3-12100](https://www.intel.com/content/www/us/en/products/sku/134584/intel-core-i312100-processor-12m-cache-up-to-4-30-ghz/specifications.html) - 4 cores + hyperthreading, 3.3-4.3Ghz, Intel UHD 730
- TeamGroup Elite 2 x 16GB DDR4 3200MT/s RAM
- Asrock Z690M-ITX/ax
  - 1x Intel 1GBe
  - 1x Dragon 2.5GBe
- Corsair SF450W PSU \*
- Fractal Design Node 202 \*
- 1TB Intel 660p M.2 NVME \*\*
- 2TB Crucial P3 M.2 NVME \*\*
- 2TB Crucial SATA SSD \*\*
- 3x 10TB HGST Ultrastar HE10 SATA HDD (Used, from [goharddrive](https://www.goharddrive.com/))
  - Purchased: April 10, 2024
- Geekbench 6 Scores: ~2100 Single Core/~6500 Multi Core

\*: Gifted by a friend

\*\*: Repurposed from other computers

## Opii, the Orange Pi 3B (...again)

Previously, opie was water damaged, so he was replaced by his brother, opii.

- [Orange Pi 3B 4GB](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-3B.html)
- Rockchip RK3566 - ARM 4 cores, 0.4-1.8Ghz
- 4GB LPDDR4 RAM
- 1x Gigabit Ethernet
- 1x USB 3.0 (5Gbps)
- 128GB M.2 2230 SSD
- Geekbench 6 Scores: ~150 Single Core/~500 Multi Core
