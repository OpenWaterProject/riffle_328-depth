# Turbidity sensor prototype

## Background

There are various approaches to depth measurement (see Ref [1] below) -- float switches, arrays of open wires, optical and sonar distance sensors, etc. Of these, a relatively simple and inexpensive approach is to leverage the dielectric properties of water by making a capacitive measurement.  

The basic picture is: a capacitor's ability to store electrical energy is enhanced when it is placed in a material with a larger dielectric constant.  Water has a larger dielectric constant than air;  so capacitors placed in water can store more energy than they can when in air.  For a capacitor formed by a long pair of wires, partially submerged in liquid, the measured capacitance of the wires will increase in proportion to the length of the wires submerged in the liquid.  So, measuring the capacitance of these wires would allow us to determine the height of the liquid.

## Circuit

A direct way to measure capacitance is to rely on the fact that the amount of time it takes to charge and discharge a capacitor, when applying a fixed voltage, will increase as the capacitance increases.  A chip like the [555 timer](REF) performs exactly this function:  it will charge a capacitor to a threshold voltage, then discharge it, then repeat the cycle, outputting a pulse every time the capacitor charges to a threshold value.  If we hook up a long pair of wires as a capacitor to a 555, and measure the length of time between pulses, we then have a simple liquid level sensor:

<img src="pics/digikey_capacitive_sensing.png">

_Source: http://www.digikey.com/en/articles/techzone/2011/sep/liquid-level-sensing-is-key-technology-for-todays-systems---part-1_

## Bill of Materials

- a [555 timer]
- a 1 K Ohm resistor
- a pair of twisted wires, not electrically connected, and sealed (with e.g. hot glue) at one end:

<img src="pics/p3.jpg" width=200>

## Schematic 

<img src="pics/riffle_depth_schem_simple.png">

## Diagram for Riffle Protoboard

<img src="pics/riffle_depth_diagram.png">

## Using the Code

The code in this repo, 'riffle_depth.ino', when used with the protoboard schematic outline above and attached to a long capacitive wire pair as in the above photo, will record the average period of pulses generated by the 555 timer.  This average period is then assumed to be linearly related to the amount of the wires submerged in liquid.  

## Calibration Methods

In order to calibrate the capacitive depth sensor, measurements of the oscillator period must be made when the wires are submerged at known depths.  The easiest way to do this is by lining up the wires with a measuring tape inside a container, and recording the output of the sensor as more water is poured into the container. This method is described in detail in video accompanying the Public Lab research note here: https://publiclab.org/notes/eustatic/05-21-2016/riffing-on-the-riffle-for-a-depth-sensor-update-from-2014

<img src="pics/tape_measure.png">

Using this method, a rough relationship was established between the period, in seconds of the oscillator output (recorded on SD card) and the height, in centimeters, of the liquid level.  This relationship was then used to generate the following graph:

<img src="pics/depth_test.png">

Giving the sensor more time to equilibrate and averaging readings over longer measurement periods may improve the precision of the measurement.

## Compensating for sources of error

Capacitive measurements can be affected by several environmental factors, leading to errors in liquid level measurement.

A nice review of sources of error for capacitive depth sensors is outlined in Ref [4], [linked here](http://www.umbc.edu/cuere/BaltimoreWTB/pdf/TM_2009_003.pdf).  The researchers assess error in measured liquid level due to:

- biofilm accumulation
- changes in conductivity
- temperature
- nonlinearity

and nicely summarize the relative magnitude of these errors here:

<img src="pics/error.png">
_Source: Ref [4] below_

One simple idea for mitigating these error sources is to use _two_ capacitive sensors, with one raised a known distance above the other.  If both sensors are in the same environment and are subject to approximately the same local fluctuations in temperature, conductivity, and biofilm, then the the difference in capacitance between the two wires might be relatively constant, allowing for most sources of error be divided out.  The 555 timer comes in dual-package variants, and two 328p Riffle hardware interrupts could be used to measure these two sensors.

## Research Notes on Public Lab

- First capacitive sensor prototype at 2014 NOLA barnraising: Original workshop: https://publiclab.org/notes/laurenrae/11-24-2014/don-explains-the-theory-behind-the-depth-sensor-for-the-riffle
- Scott Eustis research note https://publiclab.org/notes/eustatic/05-21-2016/riffing-on-the-riffle-for-a-depth-sensor-update-from-2014
- Depth sensor workshop in NOLA: https://publiclab.org/notes/stevie/06-06-2016/reflecting-on-the-depth-sensor-build and https://publiclab.org/wiki/depth-flood-sensing-in-new-orleans
- Dan Beavers alternative depth sensor proposal: https://publiclab.org/notes/danbeavers/05-18-2016/depth-sensor-proposal

## References

1. Digikey's guide to depth measurement techniques: http://www.digikey.com/en/articles/techzone/2011/sep/liquid-level-sensing-is-key-technology-for-todays-systems---part-1
2. Using light sensors and a ping pong ball: http://howmuchsnow.com/waterlevel/
e
3. Vegetronix capacitve liquid level sensor: http://njhurst.com/electronics/watersensor/
Vegetronix: https://www.vegetronix.com/Products/AquaPlumb/
4. Review of calibration issues with capacitive depth sensors: http://www.umbc.edu/cuere/BaltimoreWTB/pdf/TM_2009_003.pdf

# How you can contribute

Some useful guidelines about the best way to contribute to the project (or to fork it) can be found [here](contributing.md).

# Support and Licensing 

The Riffle_328 project has been supported through [Public Lab](www.publiclab.org)'s Open Water Initiative, and is licensed under [CERN OHL 1.2](LiCENSE.md).

<a href="http://publiclab.org"><img src="pics/boots.png" width=100></a>
