---
marp: false
theme: gaia
class: invert
---

# More than an interface - SCSI vs ATA

## A paper review

\
\
\
\
Joel Tony $\quad\quad\quad\quad\quad\quad$ Nirmal Govindaraj
2021A7PS2077G $\quad\ \ \quad$ 2021A7PS0441G

---

## Introduction

| Personal Storage (PS)       	| Enterprise Storage (ES)                	|
|-----------------------------	|----------------------------------------	|
| HDDs for personal computers 	| HDDs for enterprises                   	|
| Single user                 	| Multiple simultaneous users            	|
| Home use                    	| Strong reliability - business critical 	|
| Cost is deciding factor     	| Performance is deciding factor         	|
| Used alone                  	| Used in groups                         	|

---

## Functional Requirements

### Seek performance

- Faster head movement
- Done by better actuators, larger magnets, vibration resilience


### Rotational latency

- Make platter spin faster
- Innovations trickle down from ES to PS

<!-- todo add graph -->

---

### Aggregration

- ES drives used in groups $\rightarrow$ More external vibration
- Redundant paths from host to HDD
- ATA supports atmost 2 HDD's
- SCSI controllers support more than 2 HDD's.

---

## Advantages of SCSI

- *IDE* - Programmed I/O, *SCSI* - external controller chip
- Command queue processing
- Multiple CPU support
- Variable block size $\rightarrow$ EDP
- Dual porting $\rightarrow$ controller failover

---

## Tech differences
<!-- figure 2 -->
![](figure2.png)

---

## Mechanics

### HDA (Head/Disc assembly)

- HDA for ES drives require higher tolerance to environmental factors
- 1,000,000+ hour MTBF acheived by
  - Particle filters
  - Dessicants and Active Carbon
  - Shrouding

---

### Actuator

- larger magnets - more control - faster seeks
- latch to hold actuator in place - bi-stable in ES to prevent magnetic interference

### Spindle

- TPI - Tracks/inch
- Increasing RPM can lead to
  - Read/write head going off track causing mis-read/mis-write
  - increased windage

---

## Electronics

### Control processor

- Handles read/write
- Servo bursts - Field of info interspersed among data blocks
- Info from Servo bursts are used to move the head to the target

---

### Servo processor

- Controls servo ops - rotation, speed, direction adjustment
- More TPI $\rightarrow$ More servo processing
- runout - head is unable to follow the track and stay above it
- repeatable runout - same on each rotation - caused by platter waviness, motor variation
- non repeatable - external influences
- servo processor must constantly adjust for runout

---

### Interface

- Significantly more Silicon in ES products
- ES have 2 processors
  - Control processor - Read/Write and interfacing
  - Servo processor - servo ops
- PS have only one processor
- SCSI command set is also more comprehensive and customisable 

---

## Magnetics

- Writes - data rate and latency improved by higher RPM
- Reads - Can be adverally affected by high RPM because of noiser magnetic environment (Recording stress)
- Anti ferromagnetic coupled media (AFC) reinforces magnetic field

<!-- figure 3 -->
![](figure3.png)

---

## Perfomance Differences

### Capacity

- $Power \propto (RPM)^3$
- ES use smaller platters for higher RPM and faster seeks $\rightarrow$ random access better
- But for the same capacity more platters needed $\rightarrow$ costly
- PS drives $\rightarrow$ larger platters, lower RPM $\rightarrow$ cheaper per GB
- trend towards depopulated drives (less platters) trading capacity for perfomance

---

### Random I/O

- ES have stronger seek scheduling making seek times ~3x
- on average >2x random perfomance for ES compared to PS
- lower duty cycle for ES

<!-- figure 7 to show trend and table 2 to show impact of queue scheduling -->
![](figure7.png)
![](table2.png)
---

### Rotational vibation

<!-- Use figure 8 to explain 30 rad/s^2 and 60 rad/s^2 -->

- PS drives way more susceptible to rotational vibration
- ES drives designed to work in cabinet with other drives
- Even cabinet design can affect rotational vibrations drastically (5 - 45 $rad/s^2$)

![](figure8.png)

---

### Reliability

- PS - 8 hrs/day 300 days/year, ES - 24 hrs/day 365 days/year
- AFR highly depends in design choices
<!-- Use figure 9, 10, 11 to explain formulas -->
---

- POH $\propto$ AFR
![](figure9.png)

---

- Duty cycle $\propto$ AFR (stronger correlation with more platters)
![](figure10.png)

---

- Temperature $\propto$ AFR
![](figure11.png)

---

## Related work

<!-- Use table 4, 5, 6 to illustrate related studies -->
- Many studies have been conducted comparing SCSI vs IDE
- results depend a lot on design choices
- General observation - with similar conditions SCSI perform better than IDE

---

![](table4.png)
![](table5.png)
![](table6.png)

---

## Conclusion

- In order to compare to drive models a detailed comparison of drive specifications is needed
- All the factors explained before contribute to perfomance and we cannot compare solely based on interface (ATA vs SCSI)

---
<!-- _class: lead invert-->

## Questions?