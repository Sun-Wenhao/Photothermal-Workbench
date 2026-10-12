# Editable tissue phantom inputs

The three JSON files are small procedural **model definitions**, not calculated results. On loading, PTW builds their label volumes in memory. All three inputs have `shape = [40,40,40]`, `spacing_mm = [0.5,0.5,0.5]` and nominal cell extent 20 × 20 × 20 mm.

| File / button | Geometry |
|---|---|
| `homogeneous.json` / Homogeneous | Uniform label-1 cube |
| `layered.json` / Layered | Labels 1, 2 and 3 in 1, 3 and 16 mm z layers |
| `enhanced.json` / Enhanced | Same layers; label-22 sphere centred at `[10,10,7]` mm, radius 2 mm |

## Parameters

| Material | Label | mua (mm⁻¹) | mus (mm⁻¹) | k (W/mm/K) | Volumetric heat capacity (J/mm³/K) |
|---|---:|---:|---:|---:|---:|
| Homogeneous | 1 | 0.020 | 10 | 0.000500 | 0.003600 |
| Top layer | 1 | 0.030 | 15 | 0.000370 | 0.003740 |
| Middle layer | 2 | 0.010 | 8 | 0.000210 | 0.002185 |
| Deep layer | 3 | 0.020 | 10 | 0.000500 | 0.003600 |
| Enhanced inclusion | 22 | 0.095 | 9 | 0.000500 | 0.003600 |

All supplied materials use g = 0.9, n = 1.37 and zero perfusion. Inclusion mua is the illustrative sum 0.045 + 0.050 mm⁻¹. These are starting parameters for computational phantoms, not newly measured biological material properties. `wavelength_nm = 808` records an intended wavelength; coefficients are explicit inputs and are not looked up automatically.

Layer thicknesses must sum to `shape[2] * spacing_mm[2]`. Inclusion coordinates are measured from the outer corner of the phantom; cell centres are `(index + 0.5) * spacing`. The source uses a separate zero-based voxel-coordinate convention: `[19.5,19.5,-1.5]`, directed along `[0,0,1]`. Thus it is centred above the entry face, not at `[19.5,19.5,-1.5]` mm. GUI defaults supply a 2 mm beam radius and 0.5 W total laser power.

To customize a model, copy a JSON, change geometry/material values, keep the `format` field `ptw-phantom-v1`, then use **Project → Browse → Load Model**. Each grid axis must be 4–200 cells, total cells ≤ 4 million, with positive finite spacing. Leave the GUI's **Tumor** option off when using the supplied enhanced inclusion. General material changes belong in JSON; the Materials tab displays the loaded values.

See [Instructions](../docs/USER_GUIDE.md). Source definitions may be shared under the PTW license. Place separately downloaded digital mouse inputs in a user-created `models/user/` directory and follow their own source terms.
