# Digital mouse models

No anatomical dataset is bundled with PTW. Obtain the data from the source provider and retain its attribution and terms. PTW's software license does not replace a dataset's license.

## 1. Original Digimouse atlas

- [USC Digimouse download page](https://neuroimage.usc.edu/neuro/Digimouse_Download)
- [USC Digimouse license page](https://neuroimage.usc.edu/neuro/Digimouse_license)
- Dogdas B, Stout D, Chatziioannou AF, Leahy RM. **Digimouse: a 3D whole body mouse atlas from CT and cryosection data.** *Physics in Medicine & Biology* 52 (2007): 577–587. [DOI: 10.1088/0031-9155/52/3/003](https://doi.org/10.1088/0031-9155/52/3/003).

The original atlas download describes a 380 × 992 × 208 grid at 0.1 mm resolution. Its current provider license page specifies CC BY-SA 4.0; check the provider's current conditions when obtaining and reusing it. PTW 1.5.0 does **not** directly import the original Analyze `.hdr/.img` files.

## 2. Directly supported MCX JSON representation

For PTW, use the publicly available MCX example:

- [MCX repository: example/digimouse/digimouse.json](https://github.com/fangq/mcx/blob/master/example/digimouse/digimouse.json)
- [Raw JSON download](https://raw.githubusercontent.com/fangq/mcx/master/example/digimouse/digimouse.json)
- [MCX project](https://github.com/fangq/mcx)

The example inspected for this release has `Dim = [190, 496, 104]`, `LengthUnit = 0.2` mm and labels 0–21. It differs in sampling from the original USC download. Use the value in the actual file; do not substitute 0.8 mm or transfer a spacing from an unrelated model. PTW uses the JSON's optical coefficients and its own default tissue thermal properties. These properties need review for each study and should not be assumed to have been measured for the atlas animal.

The importer supports the linked compressed unsigned-byte MCX/JData example. It is not a universal importer for every MCX geometry or JData encoding. This file includes comments and a multiline encoded payload; download it intact rather than copying selected page text. Attribute the original atlas and the MCX representation, and check the relevant upstream terms. PTW does not redistribute this file or a MCX executable.

## 3. Import into PTW

1. On the MCX file page, use **Download raw file**, or save the raw link above as `digimouse.json`. Make sure the saved file is JSON text rather than GitHub HTML.
2. Create `models/user/` inside your extracted PTW folder and save the file there. Another writable location also works.
3. Start `PTW.exe`, open **Project → Browse**, select the JSON, set **Max axis voxels = 64** for an initial run, and click **Load Model**.
4. Check the model name, grid and voxel spacing in **Project**, and examine the labels in **Slice View**. A maximum axis of 64 gives approximately 25 × 64 × 13 cells for this input. It is a coarse functional starting point, not a recommended anatomical resolution for all research.
5. Review materials, laser placement, boundary conditions, heat settings and target/probes before running. Downsampling preserves first-to-last voxel-centre distances while changing per-axis voxel spacing; small structures may disappear.
6. Use **Save Project** and retain the JSON at the same relative location when moving the project.

Optional inspection from the extracted application folder:

```powershell
.\PTWCLI.exe --inspect-model .\models\user\digimouse.json
```