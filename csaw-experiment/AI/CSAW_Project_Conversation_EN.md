# CSAW Qualifier Coursework
## English Conversation Record

**Student:** Xinyue Chen  
**AI tool:** Codex in ChatGPT Work  
**Scope:** Instructor requirements, FPGA netlist recovery, baseline simulation, WSL and GTKWave troubleshooting, classroom presentation, and GitHub submission.

This standalone English edition follows the order of the original conversation within the stated scope. Prompts and replies are translated or edited for clarity. Bracketed descriptions identify screenshots when the student sent an image. The record distinguishes work performed by Codex from checks reproduced by the student.

## 01 · Identifying the assigned challenge

**Student:** Which competition is the instructor asking us to participate in?

**Codex:** The instructor assigned the 2026 CSAW AI Hardware Attack Challenge qualifying problem as coursework. The Slack announcement called for a GitHub repository and attendance at the finalist presentations. It also requested a five-minute classroom progress update. The coursework deadline stated in the announcement was November 13 at 3 p.m. New York time.

## 02 · Understanding the safety notice

**Student:** Why does this notice appear? [Screenshot of a cybersecurity content notice.]

**Codex:** The notice indicates that a previous response was withheld after an automated cybersecurity check. The screenshot does not identify the exact cause. The original-design simulation can still be treated as a separate task using the organizer’s test vectors.

## 03 · Defining the baseline simulation

**Student:** This is an NYU CSAW assignment, and I am only running it locally. Use the recovered original Verilog netlist and the instructor’s MicroPython script to write an Icarus Verilog testbench. Check normal SPI readback, encryption, and decryption and tell me whether each test passes.

**Codex:** I ran all three checks with Icarus Verilog 12.0. SPI returned the same word, the encryption result matched the expected ciphertext, and decrypting the known ciphertext returned the original plaintext. Both cryptographic operations also passed the BUSY checks. I adjusted the netlist’s module port declaration format for simulation and packaged the baseline files.

## 04 · Locating the recovered netlist

**Student:** Which file is the Verilog netlist recovered from the binary file?

**Codex:** The recovered netlist is `rtl/qualifier_core.v` inside the baseline ZIP. The separate file `tb/spi_baseline_tb.v` drives the simulation and checks its results. Names such as `n800` are generated names for internal nets and registers.

## 05 · Running the downloaded package

**Student:** What should I do after downloading the ZIP?

**Codex:** Extract it and open a terminal in the folder containing `run.sh`. Use WSL Ubuntu to run `bash run.sh`. Install Icarus Verilog if it is missing. A successful run prints `PASS all 3 baseline checks`. Add `+vcd` to save a waveform.

## 06 · Selecting Ubuntu in WSL

**Student:** [PowerShell screenshot showing that the default WSL environment could not find Bash or sudo.]

**Codex:** List the installed distributions with `wsl -l -v` and select the installed Ubuntu distribution by name. The errors occurred before the simulation script could start.

## 07 · Installing Icarus Verilog

**Student:** What is this prompt? [Ubuntu terminal showing a sudo password prompt.]

**Codex:** Ubuntu is asking for your WSL account password. The terminal does not echo characters as you type. You are already inside Ubuntu in the project directory. After the package list updates, run `sudo apt install -y iverilog` and then `bash run.sh` in the same window.

## 08 · Confirming the local baseline

**Student:** [Terminal screenshot showing SPI readback, encryption, and decryption passes.]

**Codex:** You reproduced all three baseline checks in WSL. The simulator’s final message confirms normal completion. Run `bash run.sh +vcd` to save a waveform for review.

## 09 · Generating the waveform

**Student:** [Terminal screenshot showing that the VCD was generated and all baseline checks passed.]

**Codex:** The VCD is available at `build/spi_baseline.vcd`. Open it with GTKWave. The main signals to inspect are START, BUSY, chip select, the SPI clock, and the readback data.

## 10 · Interpreting GTKWave startup

**Student:** What does this mean? [Terminal showing GTKWave startup and the simulation time range.]

**Codex:** GTKWave loaded the waveform file. The file covers approximately 3.6 milliseconds. The terminal remains occupied while the GUI runs. Look for the GTKWave window in the Windows taskbar.

## 11 · Recovering a hidden window

**Student:** The GTKWave icon is in the taskbar, but I cannot open the window.

**Codex:** Try switching to GTKWave with Alt+Tab and maximizing it. If it remains hidden, close that GTKWave process and reopen the existing VCD file. The saved waveform can be opened without rerunning the simulation.

## 12 · Recovering the WSL GUI

**Student:** It still will not open. [Taskbar preview showing an empty window and COPY MODE warning.]

**Codex:** The empty preview is consistent with a WSL GUI display problem. Save work in other WSL sessions before running `wsl --shutdown` from Windows PowerShell. Then reopen Ubuntu and launch GTKWave with the existing VCD.

## 13 · Adding traces

**Student:** I cannot see any waveforms. [GTKWave signal browser and an empty waveform pane.]

**Codex:** The waveform file has opened. Select BUSY in the signal list and click Append. Use Zoom Fit to show the full time range. Add START and the other signals from the same list.

## 14 · Correcting the time scale

**Student:** Why am I seeing this? [Several flat copies of BUSY over a few picoseconds.]

**Codex:** The same BUSY signal was appended several times. The display is also zoomed into a very short interval before an operation starts. Fit the full time range, retain one BUSY trace, and add START alongside it.

## 15 · Identifying encryption and decryption

**Student:** [Waveform screenshot showing two START pulses and two BUSY intervals.]

**Codex:** The first START pulse corresponds to encryption and the second to decryption. Add `cs_n` and `readback[31:0]` to show when the SPI transaction takes place and when the received word becomes available.

## 16 · Reading the completed baseline waveform

**Student:** [Waveform screenshot showing START, BUSY, chip select, and readback.]

**Codex:** START initiates the operation and BUSY marks its processing interval. After BUSY falls, SPI reads the result. The encryption readback is `9CD84392`. The initial unknown value appears before the testbench has collected a word. The terminal confirms the final decryption result.

## 17 · Understanding the test status message

**Student:** I was only reporting that I saw `PASS all 3 baseline checks`.

**Codex:** That message is printed by the simulator after the testbench checks its three expected results. The preceding text-search command only displayed matching lines from the netlist.

## 18 · Explaining the project scope

**Student:** What specifically does the qualifier require?

**Codex:** The assignment starts with recovery and validation of the original FPGA design. It then calls for a separate experimental design and simulation evidence. The submission materials include Verilog, a reproducible testbench, technical documentation, and records of AI assistance.

## 19 · Distinguishing the FPGA from its bitstream

**Student:** What does it mean to recover a circuit from the instructor’s FPGA binary file? Isn’t an FPGA a chip?

**Codex:** An FPGA is a physical chip. The bitstream configures its logic and connections. IceStorm interprets that configuration and generates a Verilog netlist suitable for simulation. The netlist represents the configured circuit and typically uses generated names for internal signals.

## 20 · Distinguishing recovery from verification

**Student:** Which step recovered and verified the original circuit? Please explain the code in detail.

**Codex:** I used IceStorm before delivering the baseline ZIP. `iceunpack` converted the binary to readable configuration data, and `icebox_vlog.py` reconstructed the Verilog netlist. You verified that result when you ran `bash run.sh` in WSL. The script uses `iverilog` to compile the recovered netlist, FPGA cell models, and testbench. It then uses `vvp` to run the simulation.

## 21 · Preparing the presentation

**Student:** Please organize my work into a PowerPoint and explain it slide by slide.

**Codex:** I prepared six English slides for the classroom progress update. They cover the project scope, bitstream recovery, original-design simulation, the experimental stage, observed comparison results, and submission materials. Slide notes include short English talking points. The baseline slide uses your GTKWave screenshot.

## 22 · Understanding SPI and the netlist

**Student:** What are SPI, `pin_36`, and the numbered signals? What does it mean for a wire to connect to a register?

**Codex:** SPI transfers data serially. In this design, `pin_36` carries SCK. MOSI carries data into the FPGA and MISO carries it out. A register samples a value on a clock edge and holds it between edges. A wire introduced with a continuous assignment follows its source without storing a separate value.

## 23 · Identifying the RP2040

**Student:** What is RP2040?

**Codex:** RP2040 is a Raspberry Pi microcontroller chip. It runs the controller program and communicates with the FPGA over SPI. In WSL, the Verilog testbench simulates these controller actions.

## 24 · Reading a clocked assignment

**Student:** What does this first Verilog block mean?

**Codex:** The block runs on the rising edge of SCK. A write-enable signal determines whether the register updates. If enabled, the reset condition selects zero and the alternative selects the data input. Otherwise, the register holds its current value. The question-mark expression is a compact way to select between two values.

## 25 · Explaining the board choice

**Student:** Why was RP2040 chosen?

**Codex:** The Hackster board supplied for the challenge already uses RP2040. It can run MicroPython and drive the SPI interface, which suits the controller role. The official documentation describes the hardware division of work without identifying an exact selection criterion.

## 26 · Finding the MicroPython reference

**Student:** What do the testbench constants mean, and where is the instructor’s MicroPython code?

**Codex:** The MicroPython example is `qualifier/spi_ice40_crypto_ip_test.py` in the organizer’s repository. It represents each reference word as four bytes. The Verilog testbench stores those same values as 32-bit constants. The known plaintext is `59C359C3`, and its expected ciphertext is `9CD84392`.

## 27 · Explaining names and wiring in the recovered netlist

**Student:** Why are these particular register and wire names used, and why is pin 36 the clock?

**Codex:** The numbered names came from the netlist recovery tool. Pin 36 already serves as the FPGA’s SPI clock input in this design. The testbench connects its simulated SCK to the matching netlist port. The recovered netlist shows the connection but does not explain the board designer’s choice of pin number.

## 28 · Walking through the three baseline tests

**Student:** Explain these lines of the testbench. [Screenshots of the readback, encryption, and decryption blocks.]

**Codex:** The readback test loads a known word and uses a second SPI transfer to read that word back. The encryption test selects encryption, sends START, checks BUSY, and compares the returned word against the known ciphertext. The decryption test separately loads the known ciphertext and compares the returned word against the known plaintext. SPI transmits and receives simultaneously, so the transmitted word and `readback` refer to opposite directions.

## 29 · Preparing the GitHub upload

**Student:** How do I put these files on GitHub? What should I upload?

**Codex:** Upload the project’s source files, testbench, simulation cell model, runner, and accompanying licenses. Retain the directory structure so the runner can find its inputs. Generated files in `build/` can be recreated. The final repository should also contain the technical README and AI records.

## 30 · Preparing a spoken script

**Student:** Give me a speech to go with the PowerPoint.

**Codex:** I drafted a short English script for each of the six slides. It explains the recovery commands by their purpose, describes the three baseline checks, and guides the speaker through the waveform and measured results. The closing slide states which submission materials still need to be prepared.

## 31 · Preparing for questions

**Student:** If the instructor asks me a question, can I use voice input to get a response?

**Codex:** Send the question as text or as an audio input that reaches the chat. I can give a short English sentence suitable for speaking and a Chinese explanation if needed. I cannot continuously hear the room.

## 32 · Counting Verilog lines

**Student:** How many lines of RTL are there?

**Codex:** The recovered Verilog file has 3,241 lines. The experimental copy has 3,262 lines, a net increase of 21 lines including comments and blank lines.

## 33 · Describing the work as preliminary

**Student:** I want the presentation to describe the experimental work as an early version so I can still revise it.

**Codex:** Describe it as an initial implementation with preliminary simulation results. Mention the cases already tested and make clear that broader verification remains future work.

## 34 · Identifying the AI tool

**Student:** What model did you use earlier?

**Codex:** I identified the tool in this conversation as Codex in ChatGPT, based on GPT-6. If the course asks for the exact version or variant, record it only if it can be independently verified.

## 35 · Locating the project files

**Student:** Which file contains the experiment? Where is the original Verilog? Which one did I upload to GitHub? [Three consecutive questions.]

**Codex:** The experimental Verilog is in `csaw-experiment/rtl/qualifier_core.v`. The recovered original is in `csaw-baseline/rtl/qualifier_core.v`. The files share a name, so their parent directories distinguish them. The upload instructions pointed to the experimental directory. The repository contents had not yet been verified from the conversation.

## 36 · Checking completeness after upload

**Student:** I uploaded CSAW-Experiment. Does that complete the qualifier? What is still missing?

**Codex:** You completed the core local simulation. The upload still needs to be checked for reproducible test materials, a README covering the observed results, and AI records. The course also requires submitting the GitHub link through the instructor’s chosen channel.

## 37 · Reviewing the repository tree

**Student:** This is the GitHub repository. [Screenshot of the repository tree.]

**Codex:** The screenshot shows `rtl/`, `sim/`, `tb/`, the runner, and a license. The contents of the testbench folder and README still need to be checked. The files under `build/` include generated simulation outputs and a temporary testbench.

## 38 · Reviewing the uploaded README

**Student:** Is this correct? [Screenshots of the README.]

**Codex:** The README correctly explains the baseline recovery, how to run it, and the three verified results. Clarify that the statement about preserved logic refers to the recovered baseline. Add the experimental observations, distinguish the available baseline runner from separate comparison testing, and include a record of AI use.

## 39 · Drafting English documentation

**Student:** The instructor can access my repository. Please give me the results, reproduction notes, and AI usage record in a formal tone.

**Codex:** I drafted English text for the simulation results and the baseline running instructions. I also provided an English AI-use statement identifying the tool, its role, and the checks the student ran independently.

## 40 · Choosing an English record

**Student:** I need an English record. Can you create a new one? I want the ordinary project exchanges in order, including the assignment, recovery, baseline, troubleshooting, presentation, and GitHub submission.

**Codex:** This document provides a standalone English edition of those project exchanges. It identifies the scope, distinguishes screenshots from written prompts, and records who performed each verification step.
