# NEWPATH

[中文](README.md) | **English**

A parametric toolpath library for large-format pellet 3D printing in Grasshopper. **1.0.4 · 26 components · Rhino 8 / Python 3**

NEWPATH connects geometry slicing, path editing, print sequencing, bead preview, and G-code output on a single Grasshopper canvas. Start with a model or manually drawn layer curves, combine components to suit your design, and inspect and adjust the toolpath as you go.

## Key benefits

- **Keep process data with the path**: PATH packets carry curves, layer indices, per-point heights, bead widths, and motion information, so slicing, editing, preview, and output share the same data.
- **Design paths around construction needs**: Combine double walls, ribs, end supports, brims, and infill to adjust local details. Organise execution through grouping, continuous paths, or layer-by-layer printing.
- **Inspect the process before export**: Examine curves, pointwise motion, and bead meshes, then simulate the print-head motion described by the G-code.
- **Use it directly in Grasshopper**: One `NEWPATH.gha` provides 26 independent components with embedded algorithms and icons. Component names and descriptions are Chinese; port identifiers and categories remain English. English component names below are translations for reference.

## Download and install

Requirements: **Windows, Rhino 8, Grasshopper, and Rhino Python 3**. Validated with Rhino 8.20 / .NET 8. Mac and Rhino's .NET Framework mode have not been validated.

1. Visit [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases) and download `NEWPATH_1.0.4.zip`. If the attachment is absent, it has not yet been uploaded.
2. Extract the ZIP, save your work, close all Rhino windows, and run `安装.ps1`. The script verifies the plugin, backs up matching legacy NEWPATH components, and installs `NEWPATH.gha`.
3. Restart Rhino and Grasshopper, then place components from the six categories in the **NEWPATH** toolbar. Open `示例/双物体逐层打印.gh` in the package to see an example setup.

For manual installation, open **File → Special Folders → Libraries Folder** in Grasshopper and copy in `NEWPATH.gha`. First move out legacy NEWPATH ghuser files and `NEWPATH.Toolbar.gha` to avoid duplicate entries. No separate icons or external Python sources are needed; execution still requires Rhino Python 3.

Existing canvas instances do not upgrade automatically. Place new components and reconnect them. Hover over input ports for Chinese descriptions and default behaviour; connected inputs use incoming data.

## Workflow

- **Continuous printing**: Smart Slicer → Path Grouping → optional path edits → Continuous Path → optional PATH to G1 / PATH to G2 G3 → Path G-code → G-code Export or Upload.
- **Multiple objects, layer by layer**: slicing → optional path edits → optional PATH to G1 / PATH to G2 G3 → Layer-by-Layer Printing → Path G-code → G-code Export or Upload. Input models must share a common layer-index reference. Within each layer, paths are ordered as brim, infill, then outer walls. Supply paths that have not yet been joined into continuous paths, and connect directly to G-code generation afterwards; regrouping or continuous-path assembly may change the order.

Use Open Continuous Path for open contours, UV Slicer for layer curves along a surface parameter direction, or Pack Path for manually drawn parallel layer curves. Each component has specific input requirements; see its port tooltips.

Connect processing nodes through their **Path** data ports. `Preview` displays geometry and cannot replace the packet carrying process attributes. Recompute PATH data upstream rather than using Internalise to store it across sessions. Trigger `Run` again on Continuous Path after saving and reopening.

Before generating G-code, check millimetre units, machine travel limits, temperature, speed, and extrusion settings. Arc output requires controller support for XY-plane G2/G3. Upload uses Moonraker and requires your machine address and any required API key. `Upload` defaults to off; uploading does not start printing. Historical equipment testing is limited to the Jinshi FGF 1800 large-format pellet printer; other machines require adaptation. **Version 1.0.4 has not yet been tested in a physical print.**

## All components

The 26 components follow the six toolbar categories. Icons are displayed at 80×80; 512×512 transparent PNGs are available in the repository. All components are installed through the same GHA.

### 1 Slice

| Icon | Component | Function |
|---|---|---|
| <img src="图标/uv_slicer_path.png" width="80" height="80" alt="UV Slicer"> | **UV Slicer** | Creates conformal slices along a selected surface parameter direction, recording per-point heights and growth directions. |
| <img src="图标/smart_slicer_path.png" width="80" height="80" alt="Smart Slicer"> | **Smart Slicer** | Slices geometry using layer heights and end planes, producing paths with bead widths and per-point attributes. |
| <img src="图标/pack_path.png" width="80" height="80" alt="Pack Path"> | **Pack Path** | Packages manually drawn parallel layer curves as Path data, preserving actual layer heights and provenance. |

### 2 Path Edit

| Icon | Component | Function |
|---|---|---|
| <img src="图标/double_wall_path.png" width="80" height="80" alt="Double Wall Path"> | **Double Wall Path** | Offsets closed wall curves inward and creates double-wall paths at specified connection points. |
| <img src="图标/surface_rib_path.png" width="80" height="80" alt="Double-Track Rib"> | **Double-Track Rib** | Creates paired out-and-back ribs inside walls using location points, layer ranges, and an overhang angle. |
| <img src="图标/brim_skirt_path.png" width="80" height="80" alt="Generate Brim"> | **Generate Brim** | Adds outward brim loops around contours touching the build plane while retaining original model paths. |
| <img src="图标/skirt_path.png" width="80" height="80" alt="Generate Surrounding Brim"> | **Generate Surrounding Brim** | Creates a closed brim surrounding all models at their base. |
| <img src="图标/end_offset_path.png" width="80" height="80" alt="End Offset"> | **End Offset** | Offsets wall ends inward or outward, layer by layer, within a specified layer range. |
| <img src="图标/end_bearing_path.png" width="80" height="80" alt="End Support"> | **End Support** | Thickens double walls across specified end-layer ranges to form support surfaces. |
| <img src="图标/bottom_hatch_fill.png" width="80" height="80" alt="Zigzag Infill"> | **Zigzag Infill** | Adds zigzag bottom infill using a specified layer range and scan spacing. |
| <img src="图标/spiral_infill_path.png" width="80" height="80" alt="Spiral Infill"> | **Spiral Infill** | Adds spiral infill to selected layers of the first model group while retaining all original paths. |

### 3 Path Assembly

| Icon | Component | Function |
|---|---|---|
| <img src="图标/open_continuous_path.png" width="80" height="80" alt="Open Continuous Path"> | **Open Continuous Path** | Alternates successive open layer curves in a group and joins them into a back-and-forth printing path. |
| <img src="图标/path_grouping.png" width="80" height="80" alt="Path Grouping"> | **Path Grouping** | Repartitions paths into print blocks at specified heights, with optional colours for each block. |
| <img src="图标/continuous_path.png" width="80" height="80" alt="Continuous Path"> | **Continuous Path** | Adjusts closed-loop seams using guide points and joins slices in a group into a continuous extrusion path. |
| <img src="图标/curve_arc_classifier.png" width="80" height="80" alt="PATH to G2 G3"> | **PATH to G2 G3** | Fits lines and arcs to paths and rebuilds them within tolerance and segment-length limits. |
| <img src="图标/path_to_g1.png" width="80" height="80" alt="PATH to G1"> | **PATH to G1** | Samples Path curves by length or geometric features to create G1 paths while preserving per-point attributes. |
| <img src="图标/layer_print_order.png" width="80" height="80" alt="Layer-by-Layer Printing"> | **Layer-by-Layer Printing** | Organises printing by actual layer index, ordering brim, infill, and outer walls within each layer. |

### 4 Export

| Icon | Component | Function |
|---|---|---|
| <img src="图标/gcode_upload.png" width="80" height="80" alt="G-code Upload"> | **G-code Upload** | Saves G-code locally and uploads it to the specified printing device. |
| <img src="图标/gcode_export.png" width="80" height="80" alt="G-code Export"> | **G-code Export** | Estimates printing time and material usage, and saves instructions to a selected directory when triggered. |
| <img src="图标/path_gcode.png" width="80" height="80" alt="Path G-code"> | **Path G-code** | Generates printing instructions from continuous paths and speed, temperature, and extrusion settings. |

### 5 Analyze

| Icon | Component | Function |
|---|---|---|
| <img src="图标/gcode_motion.png" width="80" height="80" alt="Pointwise Path Motion"> | **Pointwise Path Motion** | Reads original path points and outputs coordinates, indices, and incoming motion types. |
| <img src="图标/unpack_path.png" width="80" height="80" alt="Unpack Path"> | **Unpack Path** | Splits a path packet into layer planes, curves, nominal layer heights, and original point lists. |
| <img src="图标/rounded_mesh_pipe.png" width="80" height="80" alt="Path Preview"> | **Path Preview** | Previews printed bead meshes using per-point widths and heights from Path data. |
| <img src="图标/Gcode运动模拟_gcode_simulator.png" width="80" height="80" alt="G-code Motion Simulator"> | **G-code Motion Simulator** | Parses G-code and plays print-head motion at the specified speeds, showing the travelled path. |

### 6 Utilities

| Icon | Component | Function |
|---|---|---|
| <img src="图标/print_bed.png" width="80" height="80" alt="Print Bed"> | **Print Bed** | Displays a print bed from its device name and dimensions, and outputs a world-horizontal reference plane. |
| <img src="图标/水平偏移曲面_horizontal_offset_surface.png" width="80" height="80" alt="Horizontal Surface Offset"> | **Horizontal Surface Offset** | Offsets surfaces by a horizontal distance, with an option to close them into walls. |

## Version changes

**1.0.4 (2026-10-15)** retains all 26 functions from 1.0.3 as independently registered components in one GHA. It updates continuous paths, open continuous paths, UV slicing, brim generation, G-code generation and upload, and the shared runtime. Compared with the 25 components in 1.0.2, this version includes Layer-by-Layer Printing for sequencing multiple objects by actual layer index.

All 104 inputs have Chinese descriptions; 50 fixed defaults are stored in the ports. Dynamic defaults are calculated as explained in the tooltips. G-code generation and export are placed directly as components, without expanding old ghuser input groups. Defaults follow the current algorithms; custom values from old examples are not migrated automatically.

## Validation and license

Release validation included 100 offline regressions, structural checks for 21 PATH components, and registration, new-instance, icon, source, port, and default checks for all 26 components. Representative chains covered two-object layer-by-layer printing, continuous paths with brim and infill, and open continuous paths from UV slices. Save/reopen and development-directory isolation checks were also completed.

Network upload, physical printing, and installation after an operating-system restart have not been tested for this release. These checks reflect the release environment and inputs; they do not establish compatibility with every device or canvas combination.

This repository provides bilingual documentation and icons. Installation attachments are distributed through [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases). Licensed under the [MIT License](LICENSE). The GHA embeds the Python algorithms required at runtime and does not provide source confidentiality.
