# Notes

## Abstract

- The paper sets out to do comprehensive comparison enterprise storage (ES) and personal storage (PS) with respect to HDDs
- The paper aims to explain the actual difference between ATA and SCSI drives. A common misconception at the time was that they differ only in cable interface which is not the case
- Explains the difference in mechanics, electronics and firmware

## Introduction

- PS - HDD’s for personal computers
  - Needs to be cheap and small - cost pressure from consumer
    - Does not need to handle multiple users at once
- ES - for enterprise
  - Usually configured in aggregates (groups)
  - Need to handle multiple clients at once
  - Efficiently access small portions of large data to service requests faster
  - Must be reliable, because failure can lead to business loss

So these are the main factors, the actual interface used depends on the use case. PS can use SCSI and ES can use ATA

## Requirements

### motivations

- HDD’s require better error correction, encoding schemes and servo processing (have to store and process more servo data such as actuator arm, speed, R/W head location) with increase in areal density (storage (GB) / physical size (inches)).
- We also need greater precision and noise tolerance as complexity increases
- So as complexity increases we need to maintain performance while also reducing cost

### Seek performance

- Improving seek performance, requires the head to move from one location to another
- To reduce seek time we have to improve the head performance by using more expensive components like better magnetic circuits, better actuators, faster microprocessors.
- We also have to reduce vibrational modes that can reduce performance
- Seek time should also not be reduced by random vibrational motion

### Rotational Latency

- Make the platter spin faster
- Usually these innovations are first done for ES. Once it has been tried and tested on ES its moved to PS because the cost for migration is compensated by the profits made in ES
- The graph shows the clear difference in trends
- Generally PS innovations are cost savings type like making a 7200 rpm

### Aggregation

- FC, SCSI, SAS controllers can connect more than 2 HDD’s to the same host whereas IDEC’s have max 2
- Rotational vibration - when you couple multiple HDD’s in one cabinet the vibrations produced by one HDD can affect another

### SCSI advantages

- IDE’s traditionally had programmed I/O (no DMAC/ controller chips) whereas SCSI has an external control chip to delegate work
- SCSI has command queue
- SCSI has multiple CPU support (can send data simultaneously to multiple CPU’s) ~~which can be good for redundancy~~
- SCSI can have variable block size (size other than 512 byte sectors). This is particularly useful for implementing EDP
- SCSI has dual port which means same port can have two connections so that if one fails other can takeover
- Dual porting - A single SCSI drive can simultaneously be connected to 2 different controllers

## Tech Differences

### Mechanics

#### HDA (Head/Disc assembly)

- MTBF - mean time between failure
- ES drives operate at a higher RPM, but they also need to have a higher tolerance for external disturbance
- We have to address outgassing (particles generated by components) and prevent accumulation. These are fixed by
    1. Better sealing
    2. Particle filter
    3. Desiccant (ES)
    4. Active carbon
    5. O - rings (ES)
    6. Shrouding (physically change airflow through barriers) and air flow control to cool actuator and reduce turbulence. This is also used to cool the actuator
    7. Size and stiffness of base casting and cover affect vibrations which becomes important at high rpm

#### Actuator

- Large magnets -> Better seek times but more power needed. So actuator coils need less resistance -> thicker coils, less windings
- There's a latch to hold actuator in place when not in use but its magnetic so it can cause seek performance degradation for tracks located near the latch. ES drives have a bi-stable latch to overcome this issue, but it is more expensive.
- Seek performance is not a priority in PS drives
- in ES the coil (current passed through this along with magnets controls movement of head) and bearing cartridge (used to pivot arm) are bonded independently to the arm using special epoxy that makes it securely attached to arm

#### Spindle

- TPI - Tracks per inch
- Moving faster is a tremendous engineering challenge
- Increasing RPM can lead to the R/W head going off track, causing a misread/miswrite and rotational miss.
- Windage
  - Air movement between disk and arm
  - increases with RPM
- PS drives are attached only at the base end (cantilever). ES motor shafts are attached at both the top and bottom
- Fluid bearing motors minimise runout and acoustical noise
  - Seagate was the first to design a fluid dynamic bearing motor, captured at both ends.
  - This adds to the overall cost

### Electronics

#### Control processor

- Servo bursts
  - Drive head learns about its own position, when it crosses over a servo burst (a field of information interspered among data blocks)
  - Forces the MuP to suspend all other operations and identify the position of the head
  - These servo bursts are read constantly for feedback and adjustment

#### Servo processor

- As the TPI increases, more servo processing is needed to prevent runout
- Causes for imperfect tracks
  - Motor variation
  - Platter waviness (both along the radius and circumfrence)
- repeatable runout is uniform across rotations, non repeatable is due to external influences such as vibration
- Non-repeatable runout is harder to compensate for.
- Denser platters need more servo bursts which needs faster servo processors

#### Interface

- Signifcantly more silicon in ES products
  - ASIC gate count is 2x
  - SRAM for program code is 2x
  - Permanent flash for program code is 2x
  - Data and cache SRAM was >10x

- ES interfaces support multiple hosts, i.e. it has to be aware where the request comes from, and send the data appropriately. Note that this will not be implemented independent of coalescing, queing, etc.
- ES drives protocols with a richer command set, will require a more expensive PCB design for implementation
- In order to reduce the load on the silicon, most ES drives are equipped with 2 processors for:
    1. Servo processing
    2. Interface and read/write handling
- However, most PS drives use a single processor to handle all of this

#### Memory

- Firmware for SCSI is >2x that for ATA
- SCSI command set also allows for vendor-specific code i.e its customisable

### Magnetics

#### Heads

- Writes
  - Inductive process
  - Sensitive to linear velocity
  - Higher RPM improves not only latency but also **data rate**

- Reads
  - Magneto-resitive head
  - Insensitive to linear velocity
  - can be adversally affected by higher RPM because higher RPM means noisier magnetic environment
  - SNR - signal to noise ratio - harder to achieve in higher RPM - recording stress (harder to read)

#### Materials

- Glass substrates
  - provide greater uniformity
  - Have better shock tolerancee
  - Lower density and data rate
  - Cannot be textured
    - So actuator can not land on the media.
    - This requires a landing zone at the outer edge of the disk, but the outer edge is where density and data rate is highest
- Use of anti-ferromagnetic couple media (AFC) - 2nd magnetic layer, with orientation oppoiste to the primary layer, to reinforce the magnetic orientation.

#### Manufacturing

- ES drives require longer and more comprehensive testing

## Performance Differences

### Capacity

#### Size of Platters

- $Power \propto (RPM)^3$.
- ES drives use smaller platters, to keep power use at an acceptable level (but this means more platters for same capacity)
  - Can spin faster
  - Faster seeking
- This reduces average seek time makes random access much better than PS

- PS drives use larger platters and low RPM which gives best cost per GB

#### Number of Platters

- Trend towards depopulated drives. Only one surface per platter. Saves on the cost of a head
- Fewer platters -> Lower actuator mass -> Faster seeks

### Data Rate

- The fastest ES drive will always have higher data rate than a PS drive, but new ES gens are far apart because they double capacity every gen, whereas PS release new versions for small changes as well
- The larger media size helps the PS drives follow closely in  data rate. However, they are significantly behind in IOPS due to latency

### Random Peerformance

#### Seek times

- PS is always worse than ES
- ES drives are designed from the ground up for highest random access performance

#### Seek scheduling - Queue depth

- PS drives generally have shorter queues
  - PS seek distance is closer to the theoretical 1/3 R
  - ES drives with more aggressive and sophisticated scheduling can bring this down to as low as 1/10 R
  - A ~3x improvement for duty cycle. On average we can see >2x random performance
  - This also reduces mechanical strain on the drive because seeks are smaller in ES

#### Controller overhead

- optimised by making the processor have better perfomance through custom hardware and software

### Rotational vibration

- Manifests as a decrease in performance due to accumulation of aborted writes and rotation misses
- PS drives are more susceptible to external vibrations than ES but generally in PS its the only drive in the system whereas ES are specifically designed to work in cabinets
- As TPI increases, internal rotational vibration increases and external vibrations are already there
- Some recent drives have a rotational vibration sesor, that can detect external rotation and compensate in the servo processing.
- PS limit - $30\ rad/s^2$, ES - $60\ rad/s^2$
- best cabinets - $5\ rad/s^2$, worst - $45\ rad/s^2$

### Reliability

- Most significant difference in reliablity specification is the expected power-on hours (POH)
- POH PS - 8hr/day, 300

#### Duty Cycle

- Perfomance of mechanical work the drive has to do
- Better seek scheduling leads to shorter seeks on average and therefore a lower effiective duty cycle
- Additional platters and heads incereases the failure rate, due to additional mechanical stresses and increased heat generation. Additional head-disc interfaces can release particles.

#### Temperature

- Reliability decreases with increase in ambient temperature

## Related Work

- IDE had slightly better sequential performance but significantly lagged in random performance. However, they failed to look at all the specifications of the HDDs.
- The slight advantage of the ATA drive in sequeuential performance, can be attributed to its higher density and platter size.
- The SCSI drive's advantage over the ATA drive in random performance was due to smaller platters, and all the other design choices we learnt about earlier
- Trends observered have been bringing the performance of ATA and SCSI closer together, with ATA gaining complexity as it moves closer to SCSI.

## Summary & Discussion

- One has to look at every aspect of drive performance when comparing drives
- Capacity is approx. the same for both PS and ES
- Data rate is propotional to spindle spped, areal density and platter size.
- Fast seeks cost more
  - This includes larger magnets, better bearings and stiffer actuators.
  - The challenge is to:
        1. Rapidly find the target track
        2. Stay on the track
- Protection from rotational vibration costs extra
  - Needs better motors, top covers, stiffer actuators and additional mass
- Better scheduling costs extra
  - Needs more code space, more memory for queues and algorithms
  - This is easier to do in the SCSI interface because it has traditionally had queuing and is more mature
- High reliability costs extra
  - Needs to be considered in every component and material choice along the way

## Conclusion

- Simply separating products by their external interface - ATA vs SCSI - misses many of the details and design choices that will affect system performance

## Future Work

- Some PCs and consumer electronics use disc drives that differ significantly, from the drives studied
