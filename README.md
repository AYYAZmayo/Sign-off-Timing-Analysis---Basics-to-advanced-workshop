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

2) End point- It is a point at which data is captured at a clock edge. It is usually the ouput port or the input D pin of a register. For-example in below figure the end point is D pin or data pin of the register in path 2.

![day1 4](https://user-images.githubusercontent.com/43933912/219851816-428a6084-eec1-4092-b527-aee17acc6afa.png)
similarly for below figure, for path 1 that is IN-port to OUT-port the end point is output port "OUT". For path 2 as in above picture here is D pin of the register. In path 3 the endpoint is the D-pin of the second register. In path 4 the end point is again the ouput port.

![day1 2](https://user-images.githubusercontent.com/43933912/219851973-c07bebb4-4152-40fe-879e-fe702215d596.png)

3) Combinational logic- It is a parth element that has no memory or state. It is purely based on gates like AND ,OR NOT etc.
A combinational logic block is shon in above image as pink block. Each logic block as contains different logic gates, which have different delay and transition times due to which combinal logic block has different delay and transition time. There could be more than one timing paths are possible from a single logic block. For example in below image we have logic block containg an AND gate and an OR gate. If we add two flops here more as F1 and F2 then we have two possibele timing path available between F1 and F2, as first path 1 is from F1 to AND gate then to F2 date pin. And path2 is again from F1 to OR gate then to AND gate and finally ends at F2 D pin. So, there could multiple path possible from a logic block.

![day1 5](https://user-images.githubusercontent.com/43933912/219852497-10a7398a-f85e-4bac-b099-0d919348ec73.png)

## Setup and Hold Checks
### Setup Check
It is that a stable data should be available at the D pin of a sequential device/flip flop "some time" before the clock edge which captures the data. This "some time" is characteristic of a flip flop defined in the cell/logic library and it is dependent on a technology node.

![day1 6](https://user-images.githubusercontent.com/43933912/219853139-d59a2219-8316-440b-835c-2345ff68c1cd.png)

This enforces max-delay on the data path. If the data arrives before the setup time window then there is no setup violation and correct is captured at the flop. Whereas if the data arrives late and reaches close to the clock edge then it violates the setup constraint and a setup violation occurs as shown in the below figure.

![day1 8](https://user-images.githubusercontent.com/43933912/219856128-ca382f6a-95a1-4e7f-9d37-dfb44799dc48.png)

 ### Hold Check
It is, data should  stable for some minimum time at the input of a flip flop after the clock edge that captures the data. It is characteristic of a flop and defined in the logic library and it is also dependent on a technology node.

![day1 7](https://user-images.githubusercontent.com/43933912/219864578-035c6183-a059-4adb-929c-ea7f6a1cd7bb.png)

This enforces min delay on the data path. If the data remains stable for some time (called hold time of flop) after the clock edge then there is no hold violation and capturing flop acurately captures the data. If data does not remain stable after the clock edge for sometime/hold-time then a hold violation will occur. It happen when the delay between two flops is so low that at the capturing edge when second flop is capturing the previous data, lauching flop starts lauching new data and it may over writes the previous data and as result a hold violation will occurs.

![day1 9](https://user-images.githubusercontent.com/43933912/219865801-c4c35abb-84a0-403d-b268-834e7ca9dd1f.png)

## Slack Calculation
There is two type slack one is with respect to the setup time that is called setup slack and second is hold slack which is with respect to hold check.
###  Setup Slack Calculation
setup slack is equal to the difference of data required time and data arrival time that is:
                                  Setup slack =Data Required Time - Data Arrival Time
here,
data Required Time is equal to the actual Time in which data should have arrived at the end point of a timing path. Whereas data arrival time is a time at which data is actaully arriving at the end point.
For example, in below diagram data starts from a start point and travels through a series of logic blocks and finally reaches at the end point,  then total time taken by the data in traveling from start point to endpoint is called the arrival time of the data.

![day1 10](https://user-images.githubusercontent.com/43933912/219866784-0c1663ad-cb6d-4e25-a2c1-872416da92dc.png)

A single end point can also have multiple data arrival times from a single start point depending upon the available timing paths between them e.g. in belwo figure there are two timing paths are existing between start and end points so here we have two arrival times for same end point.

![day1 11](https://user-images.githubusercontent.com/43933912/219868643-97733394-feb5-4063-ba23-af979287186b.png)

On other hand data required time is a function of required frequency at which the design is supposed to work. It is also a function of setup time of the end point flop. 
So, the setup slack is the difference of required time and arrival time of data. If the data arrives earliar then the required then our slack is postive that means our design is meeting the timing required and can operate on the rquired frequency as shown in below figure. If arrival time equal to required time then design is just meeting the frequency requirements.

![day1 12](https://user-images.githubusercontent.com/43933912/219869084-d182be0a-99f4-43d6-83e9-63a3b61a4c43.png)

If data arrives later than the required time then we going to have a negative setup slack that means we are not meeting the rewuired timing of the design and it will not work on the required frequency.So, we have to fix the setup slack. Let's suppose we have below logic path diagram.

![day1 14](https://user-images.githubusercontent.com/43933912/219869843-892997cb-9d1c-42b6-9844-92df88cb54a4.png)

Here we have setup time Fsetup of the capturing flop and Time period of Tperiod. From figure we can see that our required time is:
                                                  Required time = Tperiod - Fsetup
                                                  Arrival time = clock-to-Q delay(Tcq) + logic path delay(Tlpd)
So, for a positve slack:
                                                  Arrival time < Required time 
                                                  clock-to-Q delay(Tcq) + logic path delay(Tlpd)   <   Tperiod - Fsetup


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
