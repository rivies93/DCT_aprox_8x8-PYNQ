# DCT_aprox_8x8-PYNQ
IPs of approximations to the two-dimensional DCT (8x8) for FPGA, with overlay and test notebook in PYNQ-Z2

## Overview
This repository contains several hardware implementations of approaches to the two-dimensional Discrete Cosine Transform (2D DCT) of 8x8 blocks [1-9], packaged as IP cores for Xilinx Vivado and integrated into an overlay for the PYNQ-Z2 card. Each approach seeks to reduce the cost in logical resources and/or latency with respect to the exact DCT, at the cost of some loss of precision — a typical compromise in image/video compression applications where the error introduced is tolerable. The project includes the bitstream, the.tcl and a Jupyter notebook to load the overlay and test each approach from Python.


## How it works

1. **Upload the required files** — the Jupyter notebook, the image dataset [10], and the hardware files (`.bit` + `.tcl`) for each DCT approximation — to the PYNQ-Z2 board.
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

## References

This project implements DCT approximations from the following works:

1. R. T. Haweel, W. S. El-Kilani, and H. H. Ramadan, Fast approximate DCT with GPU implementation for image compression, vol. 40. 2016.
2. N. Brahimi, T. Bouden, T. Brahimi, and L. Boubchir, “A novel and efficient 8-point DCT approximation for image compression,” Multimedia Tools and Applications, vol. 79, no. 11-12, pp. 7615–7631, 2020.
3. C. J. Tablada, T. L. da Silveira, R. J. Cintra, and F. M. Bayer, “DCT approximations based on Chen’s factorization,” Signal Processing: Image Communication, vol. 58, pp. 14–23, 2017.
4. A. E. U. S. Potluri, A. Madanayake, R. J. Cintra, F. M. Bayer, S. Kulasekera, “Improved 8-point Approximate DCT for Image and Video Compression Requiring Only 14 Additions,” Circuits and Systems I: Regular Papers, IEEE Transactions, vol. 61, no. 6, pp. 1727–1740, 2014.
5. R. J. Cintra and F. M. Bayer, “A DCT Approximation for Image Compression,” IEEE Signal Processing Letters, vol. 18, pp. 1–10, feb 2014.
6. F. M. Bayer and R. J. Cintra, “DCT-like transform for image compression requires 14 additions only,” Electronics Letters, vol. 48, no. 15, pp. 919–921, 2012.
7. S. Bouguezel, M. O. Ahmad, and M. N. Swamy, “A lowcomplexity parametric transform for image compression,” Proceedings - IEEE International Symposium on Circuits and Systems, pp. 2145–2148, 2011.
8. S. Bouguezel, M. Ahmad, and M. Swamy, “Low-complexity 8×8 transform for image compression,” Electronics Letters, vol. 44, no. 21, p. 1249, 2008.
9. R. K. Senapati, U. C. Pati, and K. K. Mahapatra, “A low complexity orthogonal 8×8 transform matrix for fast image compression,” Proceedings of the 2010 Annual IEEE India Conference: Green Energy, Computing and Communication, INDICON 2010, no. 1, 2010.

Dataset:

10. “SIPI Image Database - Misc.”, [Online]. Available: http://sipi.usc.edu/database/database.php?volume=misc.
