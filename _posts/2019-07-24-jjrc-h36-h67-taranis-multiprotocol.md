---
layout: post
title: How to bind the JJRC H36 and H67 to the Taranis Q X7 with the Multiprotocol TX module?
summary: asd
---

Configuring Taranis Q X7 with the Multiprotocol TX module is pretty straightforward:

- go to the `MODELSEL` setting,
- select an empty row and click `Create model`,
- click `PAGE` and assign a `Model name`,
- scroll to the bottom until you find `Internal RF` setting,
- set `Mode` to `OFF`,
- go to the `External RF` and set `Mode` to `PPM`,
- `Ch. Range` set to `CH1-12`,
- go to the `MIXER` screen by clicking the `PAGE` button,
- set `CH1` to `100 Ail`,
- set `CH2` to `100 Ele`,
- set `CH3` to `100 Thr`,
- set `CH4` to `100 Rud`,
- now you are ready to go :).

Turn off your Taranis and power on your JJRC drone. Now, hold your sticks in the following positions:

H36
- left stick -- bottom-right position
- right stick -- bottom-right position

H67
- left stick -- bottom position
- right stick -- top-right position

and power on your radio. When JJRC binds, it stops blinking.

Happy flying!
