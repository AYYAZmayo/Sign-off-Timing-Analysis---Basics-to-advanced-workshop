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

Here we have setup time Fsetup of the capturing flop and Time period of Tperiod. From figure we can see that our required time is: <br />
                                                  Required time = Tperiod - Fsetup <br />
                                                  Arrival time = clock-to-Q delay(Tcq) + logic path delay(Tlpd) <br />
So, for a positve slack: <br />
                                                  Arrival time < Required time <br />
                                                  clock-to-Q delay(Tcq) + logic path delay(Tlpd)   <   Tperiod - Fsetup <br />
## Hold slack
Hold slack is the difference of data arrival time and data required time. For understanding the slack calculation lets focus on the below diagram that has a REG to REG timing path diagram.

![day1 15](https://user-images.githubusercontent.com/43933912/219871017-11f96593-8e73-4a46-8464-2e8b57d16d8d.png)

Here, <br />
                                                 Required time = Hold time of the flop(Thd) <br />
                                                 Arrival time= Tck->Q + Combo_logic_delay(Tlpd) <br />
                                                 Hold Slack = Arrival time - Required time <br />
                                                 Hold slack = Tck->Q + Combo_logic_delay(Tlpd) - Hold time of the flop(Thd) <br />
For hold slack to be postive  data Arrival time should be greater than the hold time of the flop.<br />
## SDC Overview
SDC or synopsys design constraints are one of important inputs that is provided to the STA tool for static timing analysis. These are actually a set of commands that are provided to the STA tools. For example here some set of commands/constraints based on thier functions in the STA tool:
1) Constraints for Timing- They specify the parameters that effects the operating frequency of the design examples are:
Creat_clock <br />
Create_generated_clock<br />
Set_clock_groups<br />
Set_clock_transition<br />
set_timing_derate etc.<br />
2) Constraints for Area and Power- They specify the restrictions for the area and power of the design examples are:
set_max_area<br />
set_max_dynamic_power etc.<br />
3) Constraints for Design Rules- They are the requirements of the target technolgy examples are:
set_max_capacitance<br />
set_min_capacitance<br />
set_max_transition<br />
set_max_fanout <br />
4) Constraints for interfaces- They specify the assumptions on the boundary interfaces of the design e.g.
set_driving_cell<br />
set_input_delay<br />
set_output_delay<br />
set_load etc.<br />
5) Constraints for specific modes and configurations- These are actually assumptions on the values allowed e.g
set_case_analysis<br />
set_logic_dc<br />
set_logic_one<br />
set_logic_zero <br />
6) Exceptions on design constraints-They relax the requirements set other constriants or by set by default by STA tool for analysis e.g
set_false_path<br />
set_multiplecycle_path<br />
set_disable_timing<br />
set_max_delay<br />
set_min_delay<br />
## Clocks
For STA anaylysis clcoks are defined with help of creat_clock command. This takes some extra flags that defines further attributes of the clock e.g -period is used define the clock period, -waveform is used to define the rise and falling edges of the clock as shwon in the belwo figure. Here, period of the clock is 10 ns, and -waveform{2 4} represents that first rising edge will occur at 2 ns and first falling edge will occur at 4 ns. And in the same way clock goes on. Here {c1 ck} are representing the port or wire of the design on this clock will attach. If no port/wire is defined then a virtual clock is defined which connected to the design clock ports.

![day1 16](https://user-images.githubusercontent.com/43933912/219875735-c3db3d81-512f-4fa3-ba62-5e0f62ae2809.png)

A single clock waveform can also have multiple rising and falling edges in the same clock period as shown in the below figure.

![day1 17](https://user-images.githubusercontent.com/43933912/219876104-bafc045a-1c7d-48d8-a5ec-db5fac4d297b.png)

But there is a condition on these multiple rise and fall edges that is in one full clock period there should even number of rise and fall edge e.g
![day1 18](https://user-images.githubusercontent.com/43933912/219876157-0992b903-9f6c-4030-9cbd-afb843d29138.png)

It is always not necessary to -waveform option while defing the clock. We can also skip this option. If we skip this -waveform then the rise and fall edge of the clock will occur at 0 and 50% duty cycle. Formexample in 10ns time period clock we will have a rise edge at 0ns and falling edge at 5ns respectively.

![day1 19](https://user-images.githubusercontent.com/43933912/219876315-35b7bcd8-3c0a-45d8-8da8-60fc70660c2b.png)

 If in an IP we have two clocks which provided to one net of the IP using a MUX select as shown in below figure. In such a case we can perform STA of this IP by providing an extra flag -add switch to the creat_clock command.

![day1 20](https://user-images.githubusercontent.com/43933912/219878596-c56d845e-8a31-4773-a57b-625f66cdd872.png)


## Generated Clocks
Generated clocks are the clocks which are created inside the design and they are usually divided or multiplied version of the primary clock of the design. So, to define such clocks create_generated_clock command is used. There are some extra options are also being used in the below figure along with the command create_generated_clcok.
1) -devide_by 2 
This is actually provided to tell the STA tool that the generated clock has a frequency devide by 2 or its period is multiplied by 2. 
2) -source C1 
This is telling the STA tool the generated clock has been generated from a clock source C1 (that is primary clock)
3) -master_clcok CLK1
If our design has more then one clocks such as CLK1,CLK2, CLK3 connected to the net C1, then out of these STA tool only follows CLK1 out of them.
4) {GC2 GC1}
GC1 and GC2 are the gnerated clcoks nets.

![day1 21](https://user-images.githubusercontent.com/43933912/219880072-e8bc1ac4-a035-4b70-bcfe-fb6d8416c75e.png)

For defining the source clcok net, any net can be used along the to the generated clocks as long as there is a connection exists to the main clcok net C1.

![day1 22](https://user-images.githubusercontent.com/43933912/219881027-c9949990-29d3-47a5-89f2-ec53a52a4a5c.png)

For generating complex generated clocks from the source clock an extra flag (-edges) is used in which odd number of edges are provided of the source clock. For example in below figure DIV3 is gnerated clock which is being generated from the source SYSCLK. Here -edges{1 5 7} is provided which means that gnerated clock has first rise as same first rise edge of the source, then it has falling edge at 5 edge of the source clock and then it has again rising edge at the 7th edge of the source clock.

![day1 23](https://user-images.githubusercontent.com/43933912/219881360-4f93258f-99e5-472a-ba62-02df94d011ed.png)

For generating more complex wavefors an flag -edge_shift can also be used along with -edges for generated clocks as shon in below figure.It is uually used for gnerating pulses.

![day1 24](https://user-images.githubusercontent.com/43933912/219881544-603866b4-0848-4fa8-8d95-9aa8fcf88306.png)

Here -edges {1 1 3} -edge_shift {0 2 0} means that the gnerated clock first rise will be source first edge and it shifted by 0ns. The again second 1 in -edge{1 1 3} means generated clock also has its falling edge at 1st edge of source clock but shifted by 2ns. And finally 3 means gnerated clock has it next rising edge at third edge of source clock shifted by 0ns and so on. In this way pulse is generated from the source clock.<br />
In some cases generated clock has two possible timing paths from the source clock one could be sequential and second could be combinational. As STA tool mostly are pessimistic so they prefferably follow the sequential path. So in order to check the timing or latency along the combination path too we can provide an extra flag -combinationa in the create_generated_clcok.

![day1 25](https://user-images.githubusercontent.com/43933912/219881950-61fb08d4-5549-4233-95e9-9cfc1b1cfd81.png)

## Port Delays and Boundary Constraints
Port delays are defined by two commands 
1) set_input_delay
2) set_output_delay
Whene we have design then this need to connect to the outside world in this case the flop that are connected to the input port/output port via some logic, then in order to calculate delay inside of the design STA tool need port delays.<br />
For example for input port to flop path delay calculation we need to define the set_input_delay as 4ns as shown in below figure.

![Input_delay](https://user-images.githubusercontent.com/43933912/219882980-d1ecec2e-587b-46f6-995c-d7cc60fb5931.png)

Similarly, for flop to output port path delay calculation we need to provide set_output_delay constraint.

![Output_delay](https://user-images.githubusercontent.com/43933912/219883095-eff9ef83-818c-4c8b-8d28-c834a99f29b9.png)

Along with these commands additional flags such as -clock is also provided which tells the tool it calculate timing with respect to which clcok. Another flag that is -add_delay which is used incase we have to define port delays with respect to more then clocks. For setup check -max and for hold -min flags are also used respectively.
Additional boundary constraints are also provided to the STA tool to define the input transition specification as the input comming from the outside of the design so tool needs input/output transition specifications to accurately calculate the timing delays.<br />
Input transition specification can be provided by following methods:
1) set_drive -this specifies the resistance value at the boundary 
2) set_driving_cell - this specifies the cell driving the port and from which input transition time can calculated.
3) set_input_transition - It directly specifies the transition time value.

![day1 26](https://user-images.githubusercontent.com/43933912/219883606-da9e7f3f-72ab-4da3-8a87-fe386b2f7a08.png)

Similarly for calculating transition time at the output port following methods are used to specify the transition time in terms of load driven from the output port.
1) set_port_fanout_number - Here load is specified by the number fanout pins
2) set_fanout_load - Here load is specified in the number of standard cell/buffers
3) set_load - Here load is specified in terms of capacitance.

![day1 27](https://user-images.githubusercontent.com/43933912/219883845-ce01eb5d-82e3-43cc-9266-36a62b2647a7.png)

## Day-1-Labs
### OpenSTA Introduction
OpenSTA is a gate level static timing verifier. As a stand-alone executable it can be used to verify the timing of a design using below standard file formats.<br />
* A Gate level netlist in Verilog 
* Liberty library
* SDC timing constraints
* SDF delay annotation
* SPEF parasitics<br />
OpenSTA is a Static Timing analysis (STA) tool. An STA tool takes design, standard cell and SDC constraints as input and perform timing checks on the design.
It calculates Delay based on Integrated Dartu/Menezes/Pileggi RC effective capacitance algorithms. It can perform different timing checks on the verilog netlist for example:
* Report timing checks -from, -through, -to, multiple paths to endpoint
* Report delay calculation
* Check timing setup etc.
When we launch the openSTA tool executable in the terminal we can get the list of possible commands by typing help in the command prompt.

![day1 28](https://user-images.githubusercontent.com/43933912/219910088-2fa59ee3-527c-45f8-9012-4701a95b35fa.png)

### Inputs to openSTA
For performing a timing analysis using openSTA we have to provide following inputs to the tool:
1) Standard Cells Library
* It is provided in .lib format using read_liberty command
![day1 30](https://user-images.githubusercontent.com/43933912/219916453-56dbe784-960b-4ec4-8d07-8a159ac1c925.png)

2) Design
* Usually provided in Verilog format using read_verilog command e.g we have a design netlist named simple.v. It contains a number of standard cells

![day1 31](https://user-images.githubusercontent.com/43933912/219916635-737d01ca-8f8d-4013-b26f-883ab948e29c.png)

3) SDC timing constraints
* Timing contraints are provided in sdc file format using the command read_sdc simple.sdc

![day1 32](https://user-images.githubusercontent.com/43933912/219916715-119b8825-cc55-468a-bc75-4745c4bf9ecf.png)

### Constraints Creation and OpenSTA Runscript
#### Constraints Creation
Contraints are created using sdc commands a list of essentail contrains is given here:
1) Defining clocks- Lets define a clock with period 50 on port tau2015_clk <br />
` create_clock –period 50 –name tau2015_clk [get_ports tau2015_clk]`<br />
2) IO delays- Primary ports are defined with delays with associated clock: tau2015_clk<br />
`set_input_delay 5 –max –rise [get_ports inp1] –clock tau2015_clk`<br />
`set_output_delay -10 –min –fall [get_ports out] –clock tau2015_clk`<br />
3) Input transitions by environmental factors<br />
`set_input_transition 10 –min –rise [get_ports inp1]`<br />
4) Capacitive load on output pin<br />
`set_load –pin_load 4 [get_ports out]` <br />
#### runScript
In runscript you can define all the commands you want to run in the openSTA tool. Tool will execute each command sequentially in order. There are some commands which are executed in parallel in some cases. Runscript is in tcl format.

![image](https://user-images.githubusercontent.com/43933912/219918372-b242c80e-240e-416d-8b75-3b89d5a8ce1a.png)

Running the openSTA using command “sta run.tcl -exit | tee run.log” for simple.v netlist.

![day1 33](https://user-images.githubusercontent.com/43933912/219920677-2ad426ee-afcb-4134-98d5-642bcc9a2e9e.png)

This generates a set of timing reports in the run.log file where we can analyze whether the design slack is met or on not on the required frequency.

![day1 34](https://user-images.githubusercontent.com/43933912/219920960-3a282b35-94e9-4d1b-af6d-7fa4d3062330.png)

# Day-2
## Other Timing Checks
Other then conventional setup and hold checks we also have some timing checks that are crucial for timing verification of a design, these are
* clock gating check - these are checked on the "en" pin of the clock gating. It could be on AND , OR or any  other gate base clock gating.
* Recovery and Removal checks- Like setup and hold we also have recovery and removal checks that corresponds to async pin like clr pin or reset of a flop. They corresponds to the deassertion of reset with respect to the clock edge. 
* Data to Data checks- They are performed base upon data pins of a cell.

![day2 1](https://user-images.githubusercontent.com/43933912/219922189-34b5ec02-1865-4165-8941-082214607501.png)

## Design Rule Checks
We also have some Design rule checks that correspond to the technology node. They are of following types
1) Slew/Transition anaylsis check
There is of two type slew/transion times in design one is fall-slew and second is rise slew time.
* Fall-slew is the time taken by the signal in falling from 70% of vdd to 30% of vdd.
* Rise-slew is the time taken by the signal in rising from 30% of vdd to 70% of vdd.

![day2 2](https://user-images.githubusercontent.com/43933912/219922485-05620bc3-a59a-4089-b8e6-33b031d06d0f.png)

So, why these check are needed? It because cells present in technology library they are have a particular transition time or slew. STA tool make sure that the slew in our design should not violates the any standard cell slew otherwise the cell will not work properly and give incorrect delays. In STA there is a specific constraint that we can provide in order to ensure that these slew/transition value have to be between some Max and min values.

2) Load Analysis check
Here we can specify what could be maximum or minimum capacitance on the ports or net. And we can also specify the fanout load on ports and output pins.

3) Clock Skew analysis check
In STA we also check clock skew analysis. Slew and skew are two different things, slew means the transition time or how much time taken by the signal from 1 to zero that means fall time or from 0 to 1 that is rise time. Whereas skew is the difference in delay of the clock at different points.

![day2 4](https://user-images.githubusercontent.com/43933912/219931971-4edd83ca-b512-45e7-acc7-2779a90c224b.png)

For example in above  figure the CK_L the lauch clock arrives earliar because of single buffer delay whereas CK_C the capture clock arrive later due to three buffer delays. So, there is a difference in delay of launch clock and caputre clock at different points. If capture clock has more delay then launch clock then the skew is +Ve as in below figure. 

![day2 4](https://user-images.githubusercontent.com/43933912/219931977-d8219db2-17d3-4540-885a-bddb62c7018b.png)

On the other hand if launch clock CK_L has more delay then the capture clock CK_C then the clock skew would be -Ve.

![day2 5](https://user-images.githubusercontent.com/43933912/219932112-78afbc14-8880-4f08-91bd-1b40ba143694.png)

4) Pulse width Checks
Minimum pulse width check is performed to ensure that the clock is wide enough to get a stable output from a cell/flop. That means that there is a minimum pulse width is required at the pin of flop to operate properly and provide a stable output. Lets suppose we have an ideal clock as shown in below figure, when the clock travels through the path it's pulse width shrinks. As in below figure the fall edge of the clock slightly shrinks. If this shrinkage causes the width to reduce below a specified threshold value we can have a minimum pulse width violation 

![day2 6](https://user-images.githubusercontent.com/43933912/219932779-2da6ede2-ae13-4d8d-96ac-3dde000bd091.png)

## Latch Timing 
In flop base designs data is launched and captured at the clock edges. In latch base design Latch is level sensitive, data captured or lauched for complete complete level of the clock. Latch base design provide more timing flexibility as shown in below figure. Here we have +Ve level latch that opens at the positve edge and remains open till the falling edge of the clock that is it remains transperant and can receive the data that much extra time as compare to flop base designs which only captures the data only at the edge of clock.

![day2 7](https://user-images.githubusercontent.com/43933912/219935720-afd6764d-99a5-4120-84b4-97ec4f852e8f.png)
One of important feature that for Latch base designs can be used is time borrowing. Like in belwo figure if we have large logic delay in between the first flop and Latch and logic delay is less in between latch and second flop. Then to handle the large delay of first logic block the latch borrows time from the second block by keeping itself transparent even after the positve edge of clock. 

![day2 8](https://user-images.githubusercontent.com/43933912/219936224-94eb20d0-f3f8-4d25-8457-3c6dc4e5f7b6.png)
 
## STA Text Report
STA tool does the analysis and converts the logic into nodes, cells and arc for reporting as shown in belwo diagram. Here Clock pins and input output ports cells like flop are acts as nodes and the arrow from a input pin to output pin is representing the cell timing arc similarly we can have an arrow from ouput pin to input pin of next cell it is called a net arc.

![day2 9](https://user-images.githubusercontent.com/43933912/219937027-4318181f-f7ee-4875-8431-b6ad1ddefb59.png)

Each of these nodes and arcs are represented into STA text report along with start point, end point, transition time, cell delays and slack.

![day2 10](https://user-images.githubusercontent.com/43933912/219937306-adbbb773-d71a-414d-b655-2c9de0681248.png)

## Day-2 Labs
### Liberty Files
The liberty file of .lib file is an ASCII representation of the timing and power parameters associated with any cell in a particular semiconductor.
technology. The .lib file contains timing models and data to calcute following:
* I/O delay paths
* Timing check values
* Interconnect delays
If we open a .lib file then we can have below information like:
* Name of cell library, cell technology, units
![day2 11](https://user-images.githubusercontent.com/43933912/219938259-cb4fbdc5-9eed-4e5a-b4e6-32a1769cf423.png)

* Information about each standard cell like its cell name, power pins, leakage power.
![day2 12](https://user-images.githubusercontent.com/43933912/219938332-1cea0a94-24f9-435c-a125-5ed922dae6e8.png)

* Pin information of each cell like:

![day2 13](https://user-images.githubusercontent.com/43933912/219938367-ef739003-e76a-4c33-b418-0e210d86719e.png)

In SDC constraints the command for reading liberty file is `read_liberty`.
### SPEF

A SPEF (Standard Parasitic Exchange Format) file describes parasitic information of the design.Users would never create this file manually. It is automatically generated by the tool. It is mainly used to pass parasitic information from one tool to another.A Typical SPEF File has 4 main sections:
* Header
* Name Map
* Top Level Ports
* Parasitic description <br />
Command for spef parsing is `read_spef` <br />
### Timing Reports
In openSTA `report_checks` command is used to report timing on the design. e.g in this Lab we are running STA for a netlist design simple.v using simple_max.lib and simple_min.lib. The run.tcl is given below:<br />
`read_liberty -min simple_min.lib`<br />
`read_liberty -max simple_max.lib`<br />
`read_verilog simple.v`<br />
`link_design simple`<br />
`read_spef simple.spef`<br />
`read_sdc simple.sdc`<br />
`report_checks –group_count 5`<br />
Running the openSTA using command `sta run.tcl -exit | tee run.log` for simple.v netlist.

![day2 14](https://user-images.githubusercontent.com/43933912/219939844-279c9778-8a3c-4d4b-8c05-5a229983d382.png)

The timing report generated by openSTA is given below:
```javascript
OpenSTA 2.3.1 3c3f8f61fd Copyright (c) 2021, Parallax Software, Inc.
License GPLv3: GNU GPL version 3 <http://gnu.org/licenses/gpl.html>

This is free software, and you are free to change and redistribute it
under certain conditions; type `show_copying' for details. 
This program comes with ABSOLUTELY NO WARRANTY; for details type `show_warranty'.
Startpoint: inp1 (input port clocked by tau2015_clk)
Endpoint: f1 (rising edge-triggered flip-flop clocked by tau2015_clk)
Path Group: tau2015_clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock tau2015_clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   5.00    5.00 ^ input external delay
   0.00    5.00 ^ inp1 (in)
 113.95  118.95 v u1/o (NAND2_X1)
 119.78  238.73 ^ u4/o (NOR2_X1)
   8.32  247.05 ^ f1/d (DFF_X80)
         247.05   data arrival time

  50.00   50.00   clock tau2015_clk (rise edge)
   0.00   50.00   clock network delay (ideal)
   0.00   50.00   clock reconvergence pessimism
          50.00 ^ f1/ck (DFF_X80)
  -1.50   48.50   library setup time
          48.50   data required time
---------------------------------------------------------
          48.50   data required time
        -247.05   data arrival time
---------------------------------------------------------
        -198.55   slack (VIOLATED)
```
# Day-3
## Multiple Clocks
Uptill now we have performed STA on single clock and we performing setup and hold check on single clock. But we can also have multiple clocks in our design. In case of multiple clocks setup and hold check are also performed. For this, all the clocks are expanded on common base period(The base period is defined as the least common multiple of the clock periods involved). Then setup is performed base on the most closest launch and capture edges, that is most restrictive setup is used for setup check.

![day3 1](https://user-images.githubusercontent.com/43933912/219953687-395b40ac-88ca-4a0a-b86b-f7569f9794bd.png)

Similarly for hold check also the most restrictive hold is used. But it is chosen base on setup check. There two rules that are followed for peforming the hold check.
1) Data launched by current setup launch edge must not be captured by the previous capture edge.

![day3 2](https://user-images.githubusercontent.com/43933912/219953900-86ce007e-2439-4412-b8be-01344a8ead50.png)

2) Data lauched by next setup edge must not captured by current setup capture edge.

![day3 3](https://user-images.githubusercontent.com/43933912/219954159-be6171c8-e09b-40c5-9c36-88b20a2157f8.png)

Based on above rule we can see in belwo diagram that our hold chec is performed on previous edge for half cycle setup check.

![day3 4](https://user-images.githubusercontent.com/43933912/219955015-671511bb-4745-496a-ab06-9ff494ce6727.png)

Hence in case of multiple clocks, all the clocks are expanded to one common period and then base on this period setup check is performed on the most restrictive setup and then base on setup check the hold is performed.
## Timing Arc and Timing Sense
### Timing Arcs
There are two type of timing arcs one is cell arcs and other are net arc. Cell arc are simply the input to output connections in the cell, In each library these arc are present which tells which input is connected to which output. They describe what are input to output delays and what are the transition times. Whereas net arcs are the cell to cell connections, which describe the cell to cell delays.

![day3 5](https://user-images.githubusercontent.com/43933912/219956033-55722ac9-2205-4e84-8d10-3c7bac77cc6b.png)
* Combinational arc are the arc that are present in pure combinational element.

![day3 6](https://user-images.githubusercontent.com/43933912/219956382-365408ef-be74-4c55-b525-b75da0eed87d.png)

* Sequentail arc are the arcs that present in the sequential elements.

![day3 7](https://user-images.githubusercontent.com/43933912/219956401-3687fc00-5af0-4167-94ae-2c32eb395f92.png)

### Timing sense
There are three type of timing sense for the timing arcs
1) Positve unate
In postive unate sense if input is rising then output also rise in the same direction, similarly if input falls then output also falls in the same direction.

![day3 8](https://user-images.githubusercontent.com/43933912/219956569-0724a9f0-b0f1-4a62-ad9e-1dd142681d5d.png)

2) Negative unate
In negative unate sense if input rises then out falls in the opposite direction and vice versa.

![day3 9](https://user-images.githubusercontent.com/43933912/219956622-82ba17da-4808-4a1e-a123-913662443834.png)

3) Non Unate 
In case of non unate sense the change of output with respect to the input is un-pridictable it may rise or fall with respect to the input.

![day3 20](https://user-images.githubusercontent.com/43933912/219956690-2f3b3048-3532-4f8b-b5c1-9aba4df046d7.png)

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
