# Sign-off-Timing-Analysis---Basics-to-advanced-workshop
# Day-1
## STA Definition
Static Timing analysis is a method of verifying the timing performance of a design. In other words we can say it is a method of verifying whether a degital design can work on a particular claimed frequency. Static timing analysis is static in nature that means there is no use of dynamic simulation in STA. It means it does not involve tesbenches or test vector for simulation, because of that it is fast and can be run on large capacity designs. It clopses the whole design into minimal number of cycles and analyze once. It is exaustive in nature which means it uses mathematical models instead of test vectors and perform timing checks on all possible timing paths and scenarios. One important thing is that it does not verify the logic functionality of the design, for logical functionality we use either functional simulation or logical equilance checking. It is pessimestic and conservative in more of the cases to provide enough guard band for the design timing requirements.
It only verify the synchronous designs timing it does not verify the synchrous designs, for this we different tools that verify clock domain crossing and asynchronous checks.
## Inputs to STA
For static timing analysis we need minimum below inputs that are provide to an STA tool.
1) Netlist- A gate level netlist of the design 
2) SDC or Constraints files - Timing constrins that describe on which frequency we want to check timing of the design
3) Logic liberaries- They contains the cell delays and transition times for the logic cells like AND, or gates and flip flops etc.
## Timing Paths
## Timing Path Elements
## Setup and Hold Checks
## Slack Calculation
## SDC Overview
## Clocks
## Generated Clocks
## Boundary Constraints
## Day-1-Labs
### OpenSTA Introduction
### Inputs to openSTA
### Constraints Creation and OpenSTA Runscript
# Day-2
## Other Timing Checks
## Design Rule Checks
## Latch Timing 
## SAT Text Report
## Day-2 Labs
### Liberty Files
### SPEF
### Timing Reports
# Day-3
## Multiple Clocks
## Timing Arc and Timing Sense
## Cell Delays and Clock Network
## Setup and Hold Detailed
## STA Text Report
## Day-3 Labs
### Understanding Full REG-to-REG STA Analysis
### Slack Calculation and Review Setup Check Report
# Day-4
##  Cross Talk and Noise
##  Operating Modes and Other Variations
##  Check on Async Pins
##  Day-4 Labs
### Understanding Clock gating checks and Async Pins Checks
# Day-5
##  Clock Groups
##  Clock Properties
##  Timing Exceptions
##  Multiple Modes
##  Day-5 Labs
### Revisit Slack Computation
### Understanding CRPR and ECO inseration
