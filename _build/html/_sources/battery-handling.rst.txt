*****************************
Battery Handling Instructions
*****************************

Emergency Procedures
====================

Suspected Damage
----------------

If a battery is suspected to be damaged, but it's not on fire or actively swelling - watch the battery for at least 1 hour for the signs of critical damage (e.g. swelling, rising temperature).

Handling Battery Fire
---------------------

LiPo fire is a chemical reaction - you can't put it out with a regular fire extinguisher or sand. That doesn't mean that you don't need a fire extinguisher alltogether - it may be useful for putting out fires that the burning LiPo may start. If a battery catches fire, the best thing to do is minimize the collateral damage.

When charging, a battery may catch fire if it's not balanced properly, or battery temperature is significatntly higher or lower than room temperature (more than 30 deg. C or less than 0 deg. C) - let the battery get to ambient temperature before charging it. Good LiPos will maintain ambient temperature when charging, bad LiPos will start heating up. A hot LiPo is a red flag for a bad, dangerous battery about to catch fire. 

Collateral damage will be minimized if a battery is kept inside a LiPo Sack when it's charging and catches fire. You should always watch the battery when it's charging, and disconnect it from the charger at first signs of faulty battery.

If a battery catches on fire, disconnect it from the charger and move the battery away from any combustible objects. If possible, move the burning battery outside, because the smoke is toxic - don't inhale it!

Charging
========

Equipment
---------

- Power Supply (todo: insert image here)
- Charger (todo: image)
- Cell checker (todo: image)

Charge Rate
-----------

The battery label and specifications indicate the maximum rate at which the battery can be charged safely. Exceeding this rate will cause the battery to ignite. The charge rate is described in terms of the battery capacity C. The rate in amps is determined by multiplying the capacity in Ah by a fixed value and dropping the hour portion of the unit. For example, a 5Ah battery with a maximum charge rate of 4C can be charged at 4 * 5Ah = 20A. Although this rate is safe to use, the battery will experience the longest lifetime if charged at 1C (the numeric value of the capacity converted to amps).

Charging Instructions
---------------------

Follow these steps to charge a battery safely:

1. Power on the power supply (to allow output voltage to stabilize);
#. Plug in the charger and turn it on;
#. Connect the main battery connector to the charger;
#. Connect the balance connector to the charger;
#. Set the charger to the correct number of cells, capacity, battery chemistry (LiPo, NiMH, etc.), and charge current according to the battery specifications;
#. Verify cell voltages and perform a balance charge if voltage differences are unacceptable (>0.05V). Otherwise, perform a regular charge;
#. When charging has stopped, disconnect the balance connector and then the main connector;

Storage
=======

Discharge Rate
--------------

A similar specification exists for the maximum discharge rate of the battery and limits the ability to do extreme maneuvers. A higher discharge rate is common for applications in which there will be very high throttle usage or rapid changes in power.

Containers
----------

Batteries must be kept in fireproof bags (LiPo Sack) and the bags kept in metal containers. Batteries for air travel must be in separate fireproof bags and at storage voltage. 

Lids on metal containers should be kept open - the only exception is if the container has vents. If a battery catches fire inside a closed container, pressure inside the container will build up and it'll explode. 

Lithium Polymer Tutorial
========================

Lithium-Polymer batteries (LiPos) are highly volatile, and with improper handling they can spontaneously ignite, vent, and explode. Because of this, get an experienced battery handler to help you before you move, charge/discharge, or prepare LiPos for storage. 

LiPo batteries are classified according to number of cells, capacity, charge rate, discharge rate, and wiring plan. They are not resilient to physical damage and any puncture, crush, drop, short circuit, or other damage must be reported.

Cells
-----

A lithium-polymer cell has a maximum voltage of 4.2V and a minimum voltage of 3.7V at rest. Under load, voltages will sag and appear to be lower than these values. A cell that is charged above the maximum voltage will likely cause a fire, and a cell that is overdischarged will gain internal resistance, leading to overheating on future charges and possible fire.

Cells kept at 100% charge deteriorate faster, so batteries should only be charged a short time prior to flight (less than a day) and discharged to storage voltage soon after if not already at that level. Batteries can be stored with their cells between 3.80V and 3.85V resting.

Battery packs containing multiple cells can be wired in series or parallel. The letters S and P are used in a battery specification to indicate this. For example, we typically use 5S (i.e. 5S1P) batteries. Cells (or batteries) wired in series provide a higher voltage, while wired in parallel provide a longer runtime.

Capacity
--------

A battery is rated for a specific capacity in milliamp-hours (mAh). This value indicates how long a battery lasts at any given current draw and is equal to the current multiplied by the duration. For example, a 5000mAh (or 5Ah) battery can supply 5 amps for one hour, or 2.5 amps for 2 hours, etc.

Balancing
---------

Cells must be kept at similar voltages to avoid high current flow between cells and subsequent overheating and fire when a battery is connected. The balance connector is plugged into the charger to ensure that cells stay at similar voltages (within 0.1V at the worst).

"Not for Flight" Batteries
--------------------------

Batteries with this label have been used for a long time, and their performance likely degraded. While they're not in a bad enough condition to be thrown out, they're also not reliable enough to be used on a UAV. These batteries should be used with ground station equipment - for example, the antenna tracker. 
