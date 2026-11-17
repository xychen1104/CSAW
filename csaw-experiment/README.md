# CSAW 2026 Qualifier Experiment

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
netlist's separate port declarations. The recovered baseline netlist preserves the logic and RAM contents extracted from the bitstream. The comparison experiment was run separately. The testbench uses ENC_DEC=0 for encryption and ENC_DEC=1 for decryption, as in the organizer's MicroPython script and interface documentation. It samples MISO after the rising-edge output register settles within the high clock phase.

`LICENSE-qualifier.txt` accompanies the recovered design. The Yosys model and
its license are in `sim/`. The testbench was generated in ChatGPT Work.

## Reproduction scope

The commands above reproduce the three published functional checks using
the files in this checkout. Run them from `csaw-experiment/`. The project
uses `rtl/qualifier_core.v` as the device under test,
`tb/spi_baseline_tb.v` as the testbench, and `sim/cells_sim.v` for the iCE40
primitive models. Icarus Verilog compiles the testbench and `vvp` executes it.
A successful run prints `PASS all 3 baseline checks`.

The SHA-256 of `rtl/qualifier_core.v` in the reviewed GitHub archive is
`d92af32ff97fbba1db543777def3c112c522a9291235fc64b36c463434d125eb`. A fresh compilation of that archive passed SPI readback,
encryption, and decryption. The original bitstream is available from the
organizer's repository linked above and is required for the recovery commands.

## Previously reported comparison results

| Case | Reported observation |
| --- | --- |
| Selected encryption case | Ciphertexts differed only in bit 31. |
| One-bit-different control case | The two ciphertexts matched. |
| Functional regression | The experimental version passed the same three functional checks. |

These observations come from the earlier local comparison run. Independent
reproduction of that run is pending confirmation of the exact source
versions, the associated testbench and invocation, and the original run
outputs. The functional checks above were independently rerun from the GitHub
archive.

## AI use

See [AI use and evidence](./ai/README.md) for the interaction method, the
scope of the English record, and the status of the original records.
