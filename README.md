# Photothermal Workbench (PTW)

**Photothermal Workbench: An integrated photothermal simulation platform for tissue phantoms and digital mice**

[Download the Windows application](https://github.com/Sun-Wenhao/Photothermal-Workbench/releases/latest) · [User guide](docs/USER_GUIDE.md) · [Digital mouse models](docs/DIGITAL_MOUSE_MODELS.md)

PTW brings model preparation, optical energy deposition, transient bioheat simulation, thermal-dose analysis, and slice/3D visualization into one desktop workflow.

## Download and start

1. Open **Releases** and download **PTW_v1.5.0_Windows_x64.zip** from **Assets**.
2. Extract the entire ZIP to a writable folder. Keep `PTW.exe`, `PTWCLI.exe`, `_internal`, `models`, and `docs` together.
3. Double-click **PTW.exe**. A homogeneous tissue phantom loads automatically.
4. Choose **Simulation → Run Photothermal Simulation**. Use the output selector and **View → Slice View** or **3D View** to examine the result.

The Windows x64 release includes its Python runtime and numerical libraries. Python, MATLAB, and  GPU are **not required** to run these built-in solvers. Windows 10/11 x64 is the intended platform; the release was exercised on the maintainer's Windows host. macOS/Linux executables are not included.

**Use the named Windows ZIP in Releases.** GitHub's automatic **Source code (zip)** and **Code → Download ZIP** contain the repository's documentation and model definitions, not the application runtime.

## Included inputs and tools

| Component | Contents |
|---|---|
| Desktop application | `PTW.exe`, with a model tree, configuration tabs, slice and 3D views |
| Command-line application | `PTWCLI.exe`, with model inspection, a short self-check, and phantom runs |
| Homogeneous phantom | 20 × 20 × 20 mm cube |
| Layered phantom | 20 mm cube; 1/3/16 mm layers |
| Enhanced phantom | Layered cube with a 2 mm radius absorbing inclusion |
| Digital mouse support | Import of the linked MCX Digimouse JSON example; anatomical data downloaded separately |
| Documentation | instructions, model parameters, units, data-source guidance, dependency notices |

The phantom JSON files describe **input geometry and materials**. They contain no precomputed simulation outputs.

## Simulation scope

- Built-in voxel Monte Carlo optical deposition and an approximate diffusion option.
- Prescribed heating and optical heating with a Pennes bioheat solver.
- Optional target regions, equivalent nanoparticle absorption, and temperature probes.
- CEM43 and Arrhenius dose maps; compressed NPZ and PNG export.

The release preserves the existing interactive numerical engine while adding portable phantom loading, model-derived display surfaces, model-scale preservation, and the PTW name. It is a software distribution, not an archival reproduction of a particular manuscript's separate batch experiments. Short functional checks do not establish convergence or agreement with external solvers. See [scope and limitations](docs/USER_GUIDE.md#scope-and-limitations) before interpreting results.

## Documentation

- [User guide](docs/USER_GUIDE.md)
- [Phantom definitions and material units](models/README.md)
- [Digital mouse acquisition, attribution and import](docs/DIGITAL_MOUSE_MODELS.md)
- [Release changes](CHANGELOG.md)
- [Dependency notices](THIRD_PARTY_NOTICES.md)

## License and citation

PTW's original software license is retained in [LICENSE](LICENSE). Bundled dependencies have their own licenses, supplied with the executable release. Digital mouse data remain subject to the source provider's terms and citation requirements.

When reporting work performed with PTW, identify the software version, model source, voxel spacing, optical/thermal properties, source configuration, photon count, random seed, boundary conditions, heat-model settings, and dose assumptions. A citation template is supplied in [CITATION.cff](CITATION.cff); no publication DOI is asserted.

Report software issues through [GitHub Issues](https://github.com/Sun-Wenhao/Photothermal-Workbench/issues), including the version and a minimal input configuration.
