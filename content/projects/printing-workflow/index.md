+++
title = 'A Quick Printing Workflow'
date = '2023-11-02'
subtitle = '3d printing should be easy!'
author = 'Ivy Duggan'
draft = false
tags = [
  'workflow',
  'backups',
  'raspberry pi',
  '3d-printing',
  'dev ops',
]
+++

# Impetus

[Ender 3 v2 Neo](https://store.creality.com/products/ender-3-v2-neo-3d-printer) 300$

[Raspberry Pi 3 Model B Plus Rev 1.3](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/)

<https://www.raspberrypi.com/software/>
<https://octoprint.org/>
<https://www.prusa3d.com/page/prusaslicer_424/>
<https://www.raspberrypi.com/products/camera-module-v2/> vs c920
<https://community.octoprint.org/t/raspberry-pi-usb-power/12430>

<https://www.amazon.com/dp/B07P8337J7> ribbon cable

<https://www.amazon.com/gp/product/B07X3WBNPX> extra build plate

<https://www.amazon.com/gp/product/B00WV7GMA2> relay
<https://www.amazon.com/gp/product/B07TYQRXTK> power supply

<https://www.amazon.com/gp/product/B07ZK5F8HP> connectors
<https://www.amazon.com/gp/product/B07TGJGJGD> wires
<https://www.amazon.com/gp/product/B08YDCTHMS> wire strippers
<https://www.amazon.com/gp/product/B084GDLSCK> heatshrink

<https://www.amazon.com/gp/product/B088KJ93M3> fire alarm
<https://www.amazon.com/gp/product/B0BRB8L63T> wago

software - prusa slicer over cura, upload to api with octoprint

cheap printer - automate workflow

- add octoprint with rpi 3 -- recommend more. dont use non-sbc (recommend orange pi?)
- print case
- print camera mount
- tape power cable

need webcam

backups to nas

`nfs-common` + fstab + rsync + cron

add power switch relay
add light

Future Plans
foreshadow - lack enclosure, relocation of power supply to under table
acrylic
humidity/temp sensors
optional print failure detection - hook into homelab

<a href='plugin_list.json' download>Download Plugin_list.json</a>
bed locks <https://www.thingiverse.com/thing:5858852>
<https://www.printables.com/model/194927-ender-3v2-4040-extrusion-cap> extrusion cap
<https://www.printables.com/model/60939-frame-cap-for-v-slot-frames-ender-3-v2> extrusion cap 2
<https://www.printables.com/model/344807-klip-organizer-kabli-drukarka-3d> cable clips ender mount
<https://www.printables.com/model/263173-ender-3-v2-display-cover> screen cover
<https://www.printables.com/model/106225-modular-snap-together-raspberry-pi-2b3b3b4-case-w-> rpi case

<https://www.printables.com/model/161185-snap-fit-case-for-raspberry-pi-camera-module-2> rpi cam case
<https://www.printables.com/model/347669-v-slot-mounting-base-modular-mounting-system-gopro> modular mounting ender mount
<https://www.printables.com/model/9693-modular-mounting-system-gopro-genderaxis-changer> mounting axis switcher
<https://www.thingiverse.com/thing:2194278> modular mounting systems
