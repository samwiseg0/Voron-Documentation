---
layout: default
title: Setup a BTT Smart Filament Sensor
nav_exclude: true
---

# Setup a BTT Smart Filament Sensor

This is a guide to setup a [BIGTREETECH Smart Filament Sensor](https://github.com/bigtreetech/smart-filament-detection-module) with klipper.

BTT Smart Filament Sensor has advantages over other filament sensors as it can detect:

* Hotend jams
* Nozzle clogs
* Partial nozzle clogs
* Filament tangles
* Overcontraints of the filament path
* Under-extrusion
* Lets you seamlessly change filament

There are a few things you should do to ensure the sensor works effectively.

1. Plug the sensor into the pins for one of your free endstops. Please refer to your MCU pinout to identify the pin corresponding to the endstop you use.
1. Set your endstop pin as a pull-up (put a `^` before the pin; see the example config in the next section).
1. Ensure that the reverse bowden tube goes all the way from the sensor to the toolhead with no breaks or bends in the tube. Any open/unconstrained filament will cause it to detect false positives and interrupt your prints.
1. Sensorless homing/StallGuard is known to interfere with the sensor if the diag pin is set on the MCU where the filament sensor is connected. There should not be a jumper on the diag pin installed for the relevant endstop.
1. **(Optional)** Take the device apart entirely, and lubricate everything, _especially_ the little wheel that sits in front of the sensor. The grease that comes with the sensor is not the best, and can cause the filament to slide on the bearings instead of rotating them. That rotation is needed to drive the sensing wheel.
1. **(Optional)** Shim the interior so it can only move back and forth. See picture bellow.
    * <span style="color:green">**Green arrows** </span>indicate where the sensor should be shimmed.
    * <span style="color:Red">**Red arrows** </span> indicate what the internal body motion should be constrained to.
  ### Note:
  The last 2 steps are optional but highly recommended to prevent false positives.

<p align="center">
  <img width="400" src="./Images/btt_shim.png">
</p>

## WARNING:
**You will want to ensure that you have robust pause and resume macros. Your resume macro will need to prime the nozzle slightly so there are no gaps where the printer resumes. [Ellis has example macros](https://github.com/AndrewEllis93/Print-Tuning-Guide/blob/040d31c6daaed23c2a1a353545e7ee442a232f32/articles/useful_macros.md)**.

## Example config:
```ini
[filament_motion_sensor SFS_T0]
detection_length: 10.00 ; This can be adjusted to your desired level of sensitivity. 10 is a recommended value to prevent flow dropoff false triggers.
extruder: extruder
switch_pin: ^PG11
pause_on_runout: True
event_delay: 3.0
pause_delay: 0.5
runout_gcode:
    M117 Runout Detected!
```
