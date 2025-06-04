# Zynq UltraScale+ JTAG Boot Script

This page describes how to boot a ZynqMP SoC over JTAG using XSDB.

## XSDB Script

```tcl
connect

targets -set -filter {name =~ "PSU"}

# Enable access to PMU target

rwr csu jtag_sec ssss_pmu_sec 7
targets -set -filter {name =~ "MicroBlaze PMU"}
dow pmufw.elf
con

targets -set -filter {name =~ "Cortex-A53 #0"}
rst -processor -clear-registers
dow fsbl.elf
con

# Download system.dtb for u-boot

dow -data system.dtb 0x00100000
dow u-boot.elf
con
```
