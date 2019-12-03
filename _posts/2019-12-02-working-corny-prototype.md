---
title: "A quick update on the Corny project"
---

So, I think I made a mistake by designing Corny boards to work with two ATMega32a-PU microcontrollers. And I know this because over the weekend I rebuilt my prototype of Corny to work with Elite-C board (based on ATMega32u) mounted right between the two keyboard halves:

![Corny r2.2 mounted on a back of Galaxy S8+](/static/img/cornyr2_galaxys8plus.jpg)

As you can see, the Elite-C board is connected to the keyboard using some temporary jumper cables. And the more I use this setup the more I get inclined to redesign the boards to natively work with Elite-C. This would also simplify the process of building a Corny as builders would not have to solder in microcontrollers, additional resistors and connectors. The next prototype, which may end up being the final product, will still feature 15 jumper cables to connect the boards, and the reason I think about leaving it like that (but making routing a bit more sane) is that that would allow builders to move and adjust keyboard halves, which should improve its ergonomics.

On another related to the ergonomics note, I was able to source some [soft tactile buttons from Adafruit](https://www.adafruit.com/product/3101?gclid=EAIaIQobChMI5tPG6a6X5gIVDhgMCh0rawD_EAQYAiABEgIqTvD_BwE) which, hopefully, will address the problem of regular pushbuttons being too stiff for comfortable prolonged typing. The first boards with those buttons will be based on revision 2 of the PCB (I got five of them), but later I hope to upgrade it to r3.
