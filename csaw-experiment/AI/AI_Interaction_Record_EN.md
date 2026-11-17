# AI Interaction Record

**Project**  
NYU Tandon coursework based on the CSAW 2026 AI Hardware Attack Challenge qualifier

**Tool**  
Codex in ChatGPT Work

**Record format**  
This English record summarizes selected exchanges from the original project conversation. User requests are translated or paraphrased, and assistant responses are condensed. This document is a retrospective record rather than a verbatim transcript.

## 1. Identifying the assigned challenge

**User request**  
Which challenge did the instructor assign?

**Assistant response summary**  
The instructor assigned the qualifying challenge as coursework. The course announcement requested a GitHub repository for the submission and attendance at the finalist presentations.

## 2. Recovering and validating the original design

**User request**  
Use the recovered original Verilog netlist and the organizer’s MicroPython example to write an Icarus Verilog testbench. Check SPI readback, encryption, and decryption.

**Assistant response summary**  
Codex helped recover a simulation netlist from the iCE40 bitstream using IceStorm. It adjusted the port declaration format for Icarus Verilog and generated a testbench that used the organizer’s reference values. The three functional checks passed in simulation.

**Student verification**  
I reproduced all three checks in WSL and opened the baseline waveform in GTKWave. I reviewed the START and BUSY signals alongside the SPI readback.

## 3. Comparing the original and experimental designs

**User request**  
Proceed with the comparison described earlier. Provide instructions so I can run the simulation myself.

**Assistant response summary**  
Codex assisted with the experimental Verilog change and with comparing the original and modified designs in local simulation.

**Student verification**  
I ran the simulations locally in WSL. For the selected test case, the experimental ciphertext differed from the original by one bit. For the control input, both designs produced the same ciphertext. The experimental version also passed the organizer’s three original functional checks. These observations apply to the cases I ran in simulation.

## 4. Reviewing the submission documentation

**User request**  
Review the GitHub README and identify what still needs to be included.

**Assistant response summary**  
The existing README covered baseline recovery, simulation, and results. Codex identified the need to distinguish the original and experimental files, document the observed comparison results, and include a record of AI assistance.
