# Compression Analysis Results

## Dataset
- **Source:** RELION 3.0 tutorial (beta-galactosidase)
- **File:** `20170629_00022_frameImage.mrc` — motion-corrected micrograph
- **Download:** `ftp://ftp.mrc-lmb.cam.ac.uk/pub/scheres/relion30_tutorial_precalculated_results.tar.gz`
- **Dimensions:** 3838 × 3710 px, float32
- **Voxel size:** 0.885 Å (Falcon-III)

## Compression
- **Scheme:** bit-truncation (6-bit, no clamping)
- **Original size:** 54.3 MB
- **Compressed size:** 8.3 MB
- **Compression ratio:** 6.5×

## Analysis Results
| Metric | Value |
|--------|-------|
| FRC (avg) | 0.9974 |
| FRC (weighted) | 0.9970 |
| Correlation | 0.9969 |
| SNR | 37.85 dB |
| RMSE | 0.0651 |
| MAE | 0.0564 |
| Max Error | 0.1128 |
| Relative RMSE | 7.84% |
| Resolution (0.5 cutoff) | >Nyquist |

## Files
- `*_dashboard.png` — 2×2 summary dashboard (used in slides)
- `*_frc.png` — FRC curve plot
- `*_power_spectrum.png` — Radial power spectrum comparison
- `*_power_ratio.png` — Power preservation ratio
- `*_error_distribution.png` — Error histogram
- `*_metrics.json` — Full metrics (machine-readable)
- `*_frc.npz` / `*_radial_power.npz` — Raw analysis arrays

## Reproduction
```bash
# From the data-compression-framework repo with nix:
nix develop

# Convert MRC → TIFF (motion-corrected micrograph)
python3 -c "
import mrcfile, numpy as np
from PIL import Image
with mrcfile.open('20170629_00022_frameImage.mrc', mode='r') as m:
    Image.fromarray(m.data.copy()).save('micrograph.tiff')
"

# Compress
emencode_compress -i micrograph.tiff -s bit-truncation -o compressed/ \
  --compression-options '{"nbits": 6, "clamp_min": false, "clamp_max": false}'

# Decompress
emencode_compress --decompress -i compressed/micrograph.bittrunc.tiff \
  -s bit-truncation -o decompressed/

# Analyze
emencode_analysis --original micrograph.tiff \
  --compressed decompressed/micrograph_restored.tiff \
  --output-dir analysis/ --plots
```
