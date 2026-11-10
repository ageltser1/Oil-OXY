# Lab 8

## State Machines

---
This will be your last breadboard lab and the largest one in terms of circuit construction. Please do not [the lab](https://knowyourmeme.com/memes/please-do-not-the-cat) until the last minute.

**Avoiding the urge to procrastinate on this lab is of the utmost importance.**

Please keep in mind that you need the same clock-reset button circuitry for lab 8, so do not remove it when you are finished with lab 7!

**This lab has in-person checkoffs, which you must receive during your own lab section. Feel free to attend open lab as well if you want to ask questions or receive help from the TAs.**

---

### Steps and Points

| Step | Description                | Points |
|------|----------------------------|--------|
| 0    | Lab Assignment             | 20     |
| 1    | Build a state machine      | 60     |
| 2    | Test your circuit          |        |
| 3    | Submit your confirmation code |     |
| 4    | Demonstrate your work to a TA | 20 |
|      | **Total**                  | **100**|

\* 20% penalty for late checkoffs, bringing in liquids, or leaving an unclean/unusable station. 

> **Attention:**
>
> **You must demonstrate your working state machine to a TA during your own lab section to receive the 20 points for Step 4.** This is a large circuit — build it and verify it with Autolab *before* your lab section so that you have time to be checked off.


## Instructional Objectives

- To practice construction of state machines with behavioral Verilog.
- To practice construction of structural state machines with discrete logic.

## Introduction

In this lab experiment, you will practice building a non-trivial state machine with a next-state table and output logic. This lab builds on concepts you've learned and practiced in previous exercises. In particular, the determination of the next-state equations from a next-state table will require some thought to understand how to implement them. The lab assignment will lead you through this process and check your work.

The state machine you are to build will take time so **please start early!** You will find the specifications for the state machine in the lab assignment. You will simulate the circuit with Verilog and become familiar with its operation.

### Step 0: Lab Assignment

- Read the **entire** lab document.
- Complete the [lab assignment](submission/lab8.ipynb) in the submission folder.

### Step 1: Build a state machine

![If this diagram didn't appear, contact course staff.](src/circuit.png)

*Figure 1: Schematic of full state machine with current state, next state and output logic blocks.*

Consider the schematic diagram above, which represents the basic outline of the circuit you will construct. Some parts are intentionally left open because they will be unique to your state machine specifications. In particular:

- The connections between the outputs of the 74HC138 and the inputs to the gates must be determined for your specific state machine. (Remember, you're using an active-low 3-to-8 decoder in combination with some set of logic gates to implement the next-state values based on the current state, which are the inputs to your decoder. Use the next-state equations you implement in the lab assignment to determine what gates to use and what connections to make.)
- The connections to the inputs of the 74HC151 chips must be determined from the corresponding Boolean expressions for those chips. The first lookup table expression will be used for the first mux (output F0) and the second expression will be used for the second mux (output F1).
- ![If this diagram didn't appear, contact course staff.](src/set-reset.png)

The diagram shows that the ResetN signal should be connected to exactly one of the set or reset inputs of each individual flip-flop. The other input should be connected to a 1KΩ resistor to VDD. Using a series 1KΩ resistor is recommended to prevent accidental damage. Choose the appropriate permanent connections to implement setting the reset value for your state machine.

Each flip-flop in the diagram is implemented with half of a 74HC74 chip. Note that each chip's set and reset inputs are _active-low_. This means the "ResetN" signal is normally high and goes low, when the button is pressed, to initialize the system.

The clock and reset push button circuitry is the same as that of lab 7 and can be directly reused.
When implementing F1 and F2 make sure one is fully working before building the other. Once again use Autolab to verify.


### Tips for building your circuit

Plan your circuit before wiring it up. Build it incrementally:
- First, wire the R-C circuit and the clock button. Use a spare Schmitt-trigger inverter with a LED in series with a 150Ω resistor and a long wire as a _logic probe_ to test any signal.

![If this diagram didn't appear, contact course staff.](src/logic-probe.png)

*Figure 2: Logic probe schematic*

- Connect the inverter output to the clock input of each of the three flip-flops and attach LEDs (with series resistors) to each flip-flop’s Q output.
- Ensure each flip-flop has one unused S and R input wired high (using a resistor).
- Connect the active S and R inputs of each flip-flop to the reset button. Verify with the logic probe that the reset signal is normally high and goes low when pressed.
- Connect the flip-flop outputs to the 74HC138 select inputs (A2, A1, and A0). When the decoder is enabled, the Yx output corresponding to the reset state should be low when reset is pressed.
- Connect the appropriate Yx outputs to the AND and NAND inputs for the next-state logic.
- For each 74HC151, connect the flip-flop outputs to its select inputs (S2, S1, and S0), tie the E input low, set the I inputs according to your lookup table expressions, and attach the output to an LED (via a resistor).
- Finally, connect each NAND gate output to the corresponding flip-flop’s D input. Verify that pressing the clock button eight times cycles through all eight possible state values and returns to the original state.


#### Advice from former UTAs: <!-- UTA: Thank the following TAs for the advice: htetz -->

Make sure each of the following are fully functional before moving onto the next. Use StaticIO on the Waveforms application.

1) Verify the reset value of the flip-flops. Make sure they're in the right order, where Q2 is the MSB and Q0 is the LSB.
2) Check if the 3-bit flip-flops "clock" correctly. Reset the FFs, add test input bits into the D ports, clock it, and see if that same set of bits comes out of the Q ports.
3) Simulate 3-bit inputs into the 74HC138 as and make sure it gives the corresponding Y output.
4) Check next-state logics correctness by implementing a test signal from each Y, and making sure the output follows the lookup table.
5) Make sure you connect the appropriate DIOS, and check that all next_QX and QX tests are all fully correct. Always use StaticIO to verify.
6) When implementing F1 and F2 make sure one is fully working before building the other. Always use StaticIO to verify.

### Step 2: Test your circuit

Before using AutoLab, ensure that your circuit works with the StaticIO panel on WaveForms. Since both use the same API to interact with the AD2, any issues in WaveForms will also affect AutoLab.

Once your circuit functions as expected, connect your AD2 to the labeled _test points_ as follows:
- DIO 0: ResetN
- DIO 1: ClkN
- DIO 2: Q0
- DIO 3: Q1
- DIO 4: Q2
- DIO 5: NEXT_Q0
- DIO 6: NEXT_Q1
- DIO 7: NEXT_Q2
- DIO 8: F0
- DIO 9: F1

Ensure that the power and ground are connected to the appropriate rails. You may manually test the inputs using the StaticIO tool. When confirmed, use AutoLab to generate a confirmation code.

> **Attention:**
>
> **Download the Lab 8 testbench for Autolab for your specific operating system from the folder `testbenches/` [here](testbenches/),**
>

### Step 3: Submit your confirmation code
Save your completion code that indicates the quality of your work, and upload it as indicated by the `Submission Instructions`.

### Step 4: Demonstrate your state machine to a TA

> **Attention:**
>
> **Checkoff: Show your working state machine to a TA during your lab section to receive credit for this step.** Be prepared to:
>
> * Press the reset button to show the machine enters its reset state, then press the clock button eight times to walk through all eight states and return to the start.
> * Point out the current-state, next-state, and output-logic blocks on your breadboard, and explain how your next-state equations determined which 74HC138 outputs feed which gates.
> * Explain how you set the I inputs of each 74HC151 from your lookup table expressions for F0 and F1.
> * Show the completion code Autolab generated for your circuit.
>

### Step 5: Confirm checkoffs and clean up

Before leaving, confirm with a TA that you have completed the following steps:

* You can see that you've been checked off on Brightspace.
* Clean up your lab section, making sure that you've neatly stored all probes, cleaned up any spare wires, and turned off the light.
* **Log out of your lab station so that you don't get a logout penalty.**

Failure to follow any of these steps, or not finishing all steps of the lab by the end of your lab section, will result in a lab penalty. You can get checked off by a TA in office hours sections only after that, but only until the beginning of your lab section next week. After that, you will only receive the points you have earned thus far for the lab.

**Make sure to confirm that you have received all checkoffs on the Brightspace for your lab section before leaving.** You must also confirm that you did not receive a 20% penalty for late checkoffs, bringing in liquids, or leaving an unclean/unusable station. If you did, you must resolve the issue with your TA before leaving.

# Submission Instructions

    Failure to follow any of these instructions will result in a ZERO for that part. Please read the instructions carefully.

All expected files for lab grading will be submitted on Gradescope in a zip file. The zip file should be named <YOUR_PURDUE_USERNAME>.zip .

## Expected files to submit on Gradescope

The .zip file mentioned above should contain:

- A file called `student_submission.json` is generated by `lab8.ipynb`. This `.json` file will contain your answers from the lab assignment. Confirm the `.json` you are submitting is generated by the `lab8.ipynb` and contains your intended answers for the lab assignment. Do not rename this file.

   **DO NOT UNDER ANY circumstance edit the last cell in that file you WILL be given a **ZERO**.**

- A file called `state_machine.sv`, the file you created when completing `Submit your Verilog implementation` in the lab assignment. Make sure this file contains **top**, **hc138**, and **hc151** modules.

  Minimal example of what we are expecting the file to look like:
   ```verilog
    `default_nettype none
    module top (
      // Your top module
    );
      // Your code goes here...
    endmodule
    // Your other 2 modules
    ```

- A file called `confirmation.txt` containing the completion code generated from completing `Step 3: Submit your confirmation code` in this lab document. Make sure the completion code is on a single line.

We have provided an example submission in the `submission` folder, indicating what files you need to submit (you are not required to submit lab8.ipynb). Note, you need to submit the *.sv files as well they are not included in the submission folder as their format is given in this document. Follow the format. If you do not submit the files in the submission folder and in the format indicated, you will lose points.

