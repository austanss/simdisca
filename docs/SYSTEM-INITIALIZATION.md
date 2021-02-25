# System Startup

## Pre-run
A SIMDISCA processor zeros all registers before running any code.

## Program control
A SIMDISCA processor sets the value of MRP to `0xFFFC0000` after the pre-run sequence, and begins execution by setting bit 0 (execution flag) of MRF.