# PTW 1.5.0 — User guide

## Install and start

Download the Windows x64 asset from [Releases](https://github.com/Sun-Wenhao/Photothermal-Workbench/releases/latest), then extract it completely. Double-click `PTW.exe`. No installer, administrator rights, Python environment, MATLAB, or COMSOL license is needed for the built-in workflow. Keep `_internal` next to the executables. Use a writable folder and a display large enough for the approximately 1360 × 840 workspace; 8 GB RAM is a practical starting point for the small presets, not a guarantee for arbitrarily large runs.

## First photothermal simulation

1. In **Project**, select **Homogeneous**. The default model is 40 × 40 × 40 cells, with 0.5 mm isotropic spacing.
2. Inspect material values in **Materials**. This table is a viewer; edit a copy of the phantom JSON to change general materials, then load that copy using **Browse → Load Model**.
3. In **Photothermal**, keep the Monte Carlo deposition option for the first run. The initial GUI settings use 10,000 photons, a 500-step photon limit and seed 42. These are demonstration settings; determine convergence for scientific use.
4. In **Heat**, keep **Pennes**, laser power 0.5 W, initial/blood/ambient temperature 37 °C, one pulse, and `On/off/end s = 10 / 0 / 5`. The last number is post-heating cooling duration, so this example lasts 15 s. The default boundary is convective with 10 W/(m² K), zero metabolic heat and automatic time-step selection.
5. Select **Simulation → Run Photothermal Simulation**. The optical calculation is followed by the heat calculation. Wait until the progress/log indicates completion.
6. Choose **End-of-Heating Temperature Map**, **Peak Temperature Map**, or **CEM43 Dose Map** from **Output**. Select **View → Slice View**, then change the plane and slice index. Select **3D View** for spatial inspection. Drag the 3D view to rotate and use the mouse wheel to zoom; **Reset** restores the view.
7. In **Results**, use **Export Result NPZ** for arrays and settings, or **Screenshot PNG** for the current view. Results are written only when you choose an export path.

## Models and physical scale

**Homogeneous**, **Layered**, and **Enhanced** load the JSON inputs in `models/`. See [their specification](../models/README.md). Layering is along z. The enhanced target already exists as label 22 in the input; leave the separate **Tumor** option disabled to retain that preset exactly.

**Max axis voxels** limits the longest simulation dimension. It does not increase resolution when the original model is already smaller. Downsampling uses nearest-neighbour labels and changes per-axis spacing to preserve the distance between the first and last voxel centres. This can remove thin structures, and the resulting spacing can be anisotropic. Read the grid and voxel spacing shown in **Project** after loading.

The model-derived surface is a display representation. It does not replace voxel labels or alter the input physical scale. Interior material boundaries are easiest to inspect in **Slice View**. Digital mouse acquisition is described in [DIGITAL_MOUSE_MODELS.md](DIGITAL_MOUSE_MODELS.md).

## Sources, targets and units

| Quantity | Unit / convention |
|---|---|
| Model spacing; beam/heat radius; target dimensions and offsets | mm |
| Source/heat centre x/y/z fields | Zero-based voxel coordinates of the current simulation grid |
| Optical `mua`, `mus` | mm⁻¹; `mus` is the scattering coefficient, not reduced scattering |
| Anisotropy `g`, refractive index `n` | Dimensionless |
| Thermal conductivity `k` | W/(mm K); multiply a W/(m K) value by 0.001 |
| Volumetric heat capacity `vhc` | J/(mm³ K); multiply a J/(m³ K) value by 10⁻⁹ |
| Perfusion | s⁻¹ |
| Laser or prescribed heating power | W |
| Time and temperature | s and °C |
| CEM43 | Equivalent minutes at 43 °C |

In **Laser**, select a source row, change fields, then click **Update**, or use **Add Light Source**. `Spectrum Energy` supplies a relative source weight; total optical power is the **Heat → Laser power W** value. Source centres are not entered in millimetres. The preset source direction is +z. Angular controls describe directions in the source frame for the Monte Carlo calculation; inspect source placement after changes.

The input `wavelength_nm` is descriptive metadata. PTW does not automatically retrieve wavelength-dependent optical properties. Supply coefficients appropriate to the wavelength and material. Legacy source shape/type labels do not imply separate physical BLT/DOT or spectral solvers: this workflow uses the configured deposition model and source parameters.

**Tumor** can insert an ellipsoidal label-22 target in an imported mouse. Choose an anchor, offset and dimensions; use **Apply Tumor**. **Light Above Tumor** positions illumination relative to the target. Equivalent nanoparticle absorption applies through the target configuration, not an explicit particle-resolved geometry. In **Heat**, prescribed sources use **Add Heat Source** and **Run Thermal Simulation**. In **Probes**, choose an anatomical/model anchor and offsets, then **Add** or **Update**.

## Save and reopen

**Save Project** writes a `.ptw.json` configuration, including materials, source and thermal settings. The model path is made relative to the project file when possible. Keep the model file and project in the same relative layout when moving them. **Open Project** reloads that model and configuration. Project files do not contain simulation arrays; export NPZ separately. Loading a different preset clears previous results and source lists. **Project → New** clears the current results/source lists and reloads the selected model; it does not reset every parameter to factory defaults.

NPZ files contain numeric arrays such as `volume`, `temperature`, `temperature_end_heating`, `peak_temperature`, `temperature_frames`, `heat_time_points`, `laser_absorption_per_w`, `cem43_min`, and `arrhenius_omega`. Arrays use `(x,y,z)` order; histories use `(time,x,y,z)`. Numeric arrays can be loaded with `numpy.load(path, allow_pickle=False)`. Some descriptive metadata fields use NumPy object arrays; only enable `allow_pickle=True` for files you trust. The GUI opens project configurations, not saved NPZ result sessions.

## Command line

Open a PowerShell terminal in the extracted application folder:

```powershell
.\PTWCLI.exe --version
.\PTWCLI.exe --self-check
.\PTWCLI.exe --inspect-model .\models\layered.json
.\PTWCLI.exe --run-phantom .\models\homogeneous.json --photons 10000 --heating 10 --cooling 5 --output .\results\example.npz
```

The self-check runs short optical/thermal/dose/rendering checks on the three presets and prints a JSON status. It does not save simulation arrays. Add `--output check.json` to save the status. `--run-phantom` writes the selected run to NPZ, uses 0.5 W with Pennes defaults, seed 42, an 800-step photon limit and eight output frames. GUI and CLI defaults therefore differ; save all settings when comparing runs. The CLI uses a hidden Tk window and is intended for the same Windows desktop environment, not an untested Windows service.

## Scope and limitations

- This release packages the established interactive solver, with portable input loading and display changes. It does not assert numerical equivalence to later manuscript-specific batch scripts or external workflows.
- CEM43 and Arrhenius dose are integrated from stored output frames. Increase frame density and check convergence. The built-in Arrhenius defaults are A = 3.1 × 10⁹⁸ s⁻¹ and Ea = 6.28 × 10⁵ J/mol; they are fixed defaults, not material-specific measurements.
- `Dimension = 2D` is retained as project metadata. The numerical grid remains three-dimensional; use **Slice View** to inspect cross-sections. Some legacy presentation selectors do not select separate numerical methods.
- Display smoothing affects images, not raw exported temperature arrays. PNG screenshots are visual summaries; retain NPZ arrays for quantitative analysis.
- The **Stop** menu explains that active calculations cannot be interrupted in this version. Start with modest grids and photon counts. Closing the application during a run discards unsaved results.
- Short functional checks and the **Validate** tab are useful diagnostics, not substitutes for grid/time/photon convergence studies or experimental validation.

## Troubleshooting and storage

If DLLs or models are missing, extract the complete ZIP again and verify that `_internal` and `models` are beside `PTW.exe`. Do not run directly inside the compressed ZIP. If **User guide** has no associated viewer, open this Markdown file in a text editor or read it on GitHub.

The executables are unsigned. Check the download source and supplied SHA-256 digest according to your institution's software policy. No change to Windows security settings is required by these instructions.

If computation is slow, reduce photons and maximum grid size for a preliminary run. Higher resolution and more frames increase memory and exported NPZ size. Keep important project/model files; remove exported result files you no longer need through your usual file management. A startup failure may write `PTW_error.log` in the extracted folder. Attach the relevant error and input settings when reporting an issue, after reviewing them for local paths or private information.
