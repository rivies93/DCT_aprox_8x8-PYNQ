# DCT_aprox_8x8-PYNQ
IPs of approximations to the two-dimensional DCT (8x8) for FPGA, with overlay and test notebook in PYNQ-Z2

## Overview
This repository contains several hardware implementations of approaches to the two-dimensional Discrete Cosine Transform (2D DCT) of 8x8 blocks, packaged as IP cores for Xilinx Vivado and integrated into an overlay for the PYNQ-Z2 card. Each approach seeks to reduce the cost in logical resources and/or latency with respect to the exact DCT, at the cost of some loss of precision — a typical compromise in image/video compression applications where the error introduced is tolerable. The project includes the bitstream, the.tcl and a Jupyter notebook to load the overlay and test each approach from Python.


## How it works

1. **Upload the required files** — the Jupyter notebook, the image dataset, and the hardware files (`.bit` + `.tcl`) for each DCT approximation — to the PYNQ-Z2 board.
2. **Run the notebook.** From within it, select which `.bit` overlay to load, corresponding to the DCT approximation you want to test.
3. **Reconstruction sweep.** For each image in the dataset, the notebook reconstructs it using a varying number of transform coefficients, selected via a classic zig-zag scan order — simulating a JPEG-like compression pipeline where only the first *N* coefficients are kept per 8x8 block.
4. **Metrics export.** Quality metrics (PSNR/MSE/SSIM) are computed between each reconstructed image and its original, across all tested coefficient counts, and saved to a `.csv` file for later analysis/plotting.

## Output format

The notebook produces a `.csv` file with one row per coefficient count tested (from fewest to most coefficients kept in the zig-zag scan), with three columns:

| Column | Metric | Description |
|--------|--------|-------------|
| 1 | PSNR (dB) | Peak Signal-to-Noise Ratio, averaged across the dataset |
| 2 | MSE | Mean Squared Error, averaged across the dataset |
| 3 | SSIM | Structural Similarity Index, averaged across the dataset |

No header row is included. Example (first few rows, fewest coefficients first):

```csv
23.2994476994,655.144359863,0.522858521151
24.2534006067,537.201160879,0.595111268111
25.2808294925,431.921930342,0.646395408913
...
```

As the number of retained coefficients increases, reconstruction quality improves monotonically — PSNR and SSIM rise while MSE drops, converging toward near-lossless reconstruction as most of the block's coefficients are kept.
