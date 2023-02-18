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
In order to analyze the design STA tool breaks down the design into smaller parts that are known as timing paths. It breaks the logic/paths at ports and at between sequential logic elements. Let's understand this with help of a simple image example. Here we have a simple logic diagram which contains two input port "IN" , output port "OUT", input clock port "CLK" and some in between two flip flops or sequential elements as shwon in bewlo diagram.

![day1 1](https://user-images.githubusercontent.com/43933912/219847803-2f5f099a-5b4a-4ee2-b48f-9a2ff42aaab3.png)

The whole logic is broken down into 4 smaller parts/timing paths which described her with the help of above diagram.
1) Timing path 1- Input port to Output port timing path having some logic in between them.
2) Timing path 2- Input port to D pin of sequential element or flip flop having some logic in between them.
3) Timing path 3- REG to REG Path that is from the clock-pin of first register to the D-pin of second register having some logic in betweem them.
4) Timing path 4- REG to Output port path that is from the clock pin of a flop to the output port of the design having some logic in between them.

![day1 2](https://user-images.githubusercontent.com/43933912/219848292-5fd03767-8a0b-4097-a863-1bd4503e2d1e.png)

## Timing Path Elements
Each timing path has three basic elements.
1) Start Point- Its a point from where usally data is launched by clock edge, or where data must be available at a specific time. It is usually Input port or register clock pin. For example for path-1 in below image we have a start point as Input port "IN".

![day1 3](https://user-images.githubusercontent.com/43933912/219848714-69f8d8f4-b5dd-4319-a2d0-1a729aaed804.png)

Similarly for path 3- we have the start point as the clock pin of first register. In path 2 again we have a start point as Input port "IN". In 4th path which is from register to output port the start point is the clock pin of the register.

![day1 2](https://user-images.githubusercontent.com/43933912/219849017-93af6326-fe76-4158-aa63-53fe74c8cf81.png)

2) End point- It is a point at which data is captured at a clock edge. It is usually the ouput port or the input D pin of a register. For-example in below figure the end path is 
3) Combinational logic
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
