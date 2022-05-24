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
* Overcontraints of the filament path (Under-extrusion)
* Under-extrusion
* Lets you seamlessly change filament

There are a few things you should do to ensure the sensor works effectively.

1. The sensor is plugged into an endstop port. Please refer to your MCU pinout to identify the pin you are using. 
1. Set your endstop pin as a pull-up (put a `^` before the pin).
1. Ensure that the reverse bowden is contiguous all the way from the toolhead to the sensor. Any open/unconstrained filament will cause it to false detect
1. Sensorless homing/StallGuard is known to interfere with the sensor if the diag pin is set on the MCU where the filament sensor is connected.
1. **(Optional)** Take it entirely apart and lubricate everything especially the little wheel that sits in front of the sensor. The grease that comes with the sensor is not the best and can cause the filament to slide on the bearings instead of rotating them which is needed to drive the sensing wheel.
1. **(Optional)** Shim the interior so it can only move back and forth. See picture bellow.
    * <span style="color:green">**Green arrows** </span>indicate where the sensor should be shimmed.
    * <span style="color:Red">**Red arrows** </span> indicate what the internal body motion should be constrained to.
## Note:
The last 2 steps are optional but highly recommended to prevent false positives. 

<p align="center">
  <img width="400" src="./Images/btt_shim.png">
</p>

## WARNING:
**You will want to ensure that you have robust pause and resume macros. Your resume macro will need to prime the nozzle slightly so there are no gaps where the printer resumes. Ellis has examples on [his GitHub](https://github.com/AndrewEllis93/Print-Tuning-Guide/blob/040d31c6daaed23c2a1a353545e7ee442a232f32/articles/useful_macros.md)**

## Example config:
```ini
[filament_motion_sensor SFS_T0]
detection_length: 10.00 ; This can be adjusted to your desired level of sensitivity. 10 is a recomended value to prevent flow dropoff false triggers.
extruder: extruder
switch_pin: ^PG11
pause_on_runout: True
event_delay: 3.0
pause_delay: 0.5
runout_gcode:
    M117 Runout Detected!
```
