+++
title = 'Current Homelab Hardware'
date = '2023-12-09'
subtitle = 'The janky hardware I use.'
author = 'Ivy Duggan'
draft = false
tags = [
    'dev ops',
    'homelab',
    'linux',
    'nas',
    'networking',
    'raspberry pi'
]
header_img = ''
description = ''
toc = true
categories = []
series = []
+++

# Rolecall

Part of the modus operandi of my work in the homelab space is to re-use old hardware, because:

1. It's a fun challenge to work with sub-optimal hardware.
1. I'm cheap.

As such, here's the current information on my homelab hardware, none of which is very specialized or powerful.

## Nessie, the 2012 Macbook Pro

- [Intel Core i5-2430M](https://www.intel.com/content/www/us/en/products/sku/53450/intel-core-i52430m-processor-3m-cache-up-to-3-00-ghz/specifications.html): 2 cores + hyperthreading, 2.4-3.0Ghz
- 2 x 2GB DDR3 1333MT/s RAM
- 1x Gigabit Ethernet
- 1x Thunderbolt 2 Port
- 2TB Crucial SATA SSD - Upgraded from original 128GB SSD.
- Geekbench 6 Scores: ~500 Single Core/~750 Multi Core

## Maccy, the 2015 Macbook Pro

- [Intel Core i5-5257U CPU @ 2.70GHz](https://www.intel.com/content/www/us/en/products/sku/53450/intel-core-i52430m-processor-3m-cache-up-to-3-00-ghz/specifications.html): 2 cores + hyperthreading, 2.7-3.1Ghz
- 2 x 4GB DDR3 1867MT/s RAM
- 1x Gigabit Ethernet (using 1x Thunderbolt -> 1GbE)
- 2x Thunderbolt 2 Port (1x in use)
- 1x USB 3.0 (5Gbps)
- 128GB SSD
- Geekbench 6 Scores: ~900 Single Core/~1800 Multi Core

## Opie, the Orange Pi 3B

- [Orange Pi 3B 4GB](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-3B.html)
- Rockchip RK3566 - ARM 4 cores, 0.4-1.8Ghz
- 4GB LPDDR4 RAM
- 1x Gigabit Ethernet
- 1x USB 3.0 (5Gbps)
- 128GB M.2 2230 SSD
- Geekbench 6 Scores: ~150 Single Core/~500 Multi Core

## Raspi, the Raspberry Pi 3B+

- [Raspberry Pi 3 Model B Plus Rev 1.3](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/)
- Broadcom BCM2837B0, ARM 4 cores, 1.4GHz
- 1GB LPDDR2 RAM
- 1x Gigabit Ethernet
- 128GB Micro-SD
- Geekbench 4 (**not 6**) Scores: ~500 Single Core/~1200 Multi Core
