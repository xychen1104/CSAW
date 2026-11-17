# CSAW 2026 qualifier baseline simulation

This local simulation replays the organizer's RP2040 MicroPython example against
the Verilog netlist recovered from the original iCE40-UP5K qualifier bitstream.
The testbench uses a 50 kHz clock, transmits each 32-bit word MSB first, checks
BUSY after START and after seven additional clocks, and fails with a nonzero
exit status if a data or BUSY check fails.

## Run

Install Icarus Verilog so `iverilog` and `vvp` are on PATH. In Ubuntu or WSL:

```sh
sudo apt install iverilog
bash run.sh
```

Run `bash run.sh +vcd` to also write `build/spi_baseline.vcd`. The bundle includes
the iCE40 simulation cells from Yosys v0.48. A separate Yosys installation and
the physical Hackster board are unnecessary for this simulation.

## Verified results

Icarus Verilog 12.0 produced:

| Check | Expected | Observed |
| --- | --- | --- |
| SPI readback of 0x59C359C3 | 0x59C359C3 | 0x59C359C3 |
| Encryption of 0x59C359C3 | 0x9CD84392 | 0x9CD84392 |
| Decryption of 0x9CD84392 | 0x59C359C3 | 0x59C359C3 |

The BUSY check passed for both cryptographic operations.

## Recovery and simulation details

The bitstream came from Jason Blocklove's
[CSAW 2026 qualifier](https://github.com/JBlocklove/CSAW-AI-Hardware-Attack-Challenge-2026/tree/main/qualifier).
The uploaded bitstream matches the organizer's copy byte for byte. Its SHA-256
is `5fa5fda9768c36808780398d8e24d125d860aacc62b61af8b4c5124dd759ddbe`.
IceStorm commit `00da9c3e6ceff056248e3451dd6fd3210d6ceec9` was used:

```sh
iceunpack ice40_bitstream.bin reference.asc
icebox_vlog.py -D -s -l -n qualifier_core reference.asc > recovered.v
```

The bundled netlist uses a non-ANSI module header so Icarus accepts the recovered
netlist's separate port declarations. The recovered baseline netlist preserves the logic and RAM contents extracted from the bitstream. The experimental version was maintained separately. The testbench uses ENC_DEC=0 for encryption and ENC_DEC=1 for decryption, as in the organizer's MicroPython script and interface documentation. It samples MISO after the rising-edge output register settles within the high clock phase.

`LICENSE-qualifier.txt` accompanies the recovered design. The Yosys model and
its license are in `sim/`. The testbench was generated in ChatGPT Work.

## Simulation Results

The recovered baseline design passed three checks in Icarus Verilog:
SPI readback, encryption, and decryption. The experimental version also
passed the same baseline checks.

| Comparison case | Observed result |
| --- | --- |
| Selected encryption input | The original and experimental ciphertexts differed only in bit 31. |
| Control input differing by one bit | Both versions returned identical ciphertext. |

These results apply to the inputs tested in local simulation.

## Running the Baseline Checks

From the root of the GitHub repository, run the following in Ubuntu or WSL
with Icarus Verilog installed:

```bash
cd csaw-experiment
bash run.sh
```

The script compiles the design and runs the three baseline checks. A
successful run prints `PASS all 3 baseline checks`. To generate a waveform,
run `bash run.sh +vcd`.

The comparison results above came from a separate local simulation run.
The baseline command runs the original functional checks on the netlist
included in this folder.
