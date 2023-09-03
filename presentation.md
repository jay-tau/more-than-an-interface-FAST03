---
marp: true
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
<!-- todo HDD exploded image -->

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

<!-- _class: lead invert-->

## Questions?