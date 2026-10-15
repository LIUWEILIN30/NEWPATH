# NEWPATH

[中文](README.md) | **English**

A Grasshopper toolset for large-format pellet 3D printing. **1.0.4 · 26 components · Rhino 8 / Python 3**.

Build workflows for slicing, path editing and assembly, bead preview, and G-code generation. Component names and descriptions are Chinese; port identifiers and categories retain English names.

## Download and install

Visit [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases), choose the version, and download `NEWPATH_1.0.4.zip`. Attachments are uploaded by the maintainer; if absent, the release upload is still pending.

Extract the ZIP, save your work, close all Rhino windows, and run `安装.ps1`. The script verifies the plugin, backs up matching legacy NEWPATH components, and installs `NEWPATH.gha` to `%APPDATA%/Grasshopper/Libraries`. Restart Rhino and Grasshopper.

For manual installation, copy `NEWPATH.gha` to Grasshopper's Libraries Folder. First move out legacy NEWPATH ghuser files and `NEWPATH.Toolbar.gha` to avoid duplicate entries. Algorithms and icons are embedded. Existing canvas instances do not upgrade automatically; place new components and reconnect them.

## Version 1.0.4

All 26 components from 1.0.3 are retained as independently registered components in one GHA. Updates cover continuous and open continuous paths, UV slicing, brim, G-code generation, upload, and the shared runtime.

All 104 inputs have Chinese descriptions; 50 fixed defaults are stored in the inputs. Dynamic defaults are calculated as described in tooltips. G-code generation and export are placed directly as compiled components, without expanding ghuser input groups. Review algorithm defaults against your equipment settings.

## Workflow and requirements

Typical workflow: slicing → grouping → optional path edits → continuous paths → optional G1 or G2/G3 conversion → G-code generation → export or upload. Connect Path data ports; Preview is for inspection. Choose open-path and UV workflows according to their input requirements.

Validated on Windows Rhino 8.20, .NET 8, Grasshopper, and Rhino Python 3. Mac and Rhino's .NET Framework mode have not been validated. Embedded algorithms run in Python 3 and do not provide source confidentiality.

Recompute PATH data upstream rather than internalising it across sessions. Trigger continuous-path Run again after reopening. Upload defaults to False; installation and component placement do not start printing. Examples are included in the ZIP's `示例` folder.

Equipment testing is limited to the Jinshi FGF 1800 large-format pellet printer. Robot printers and desktop high-speed printers have not been validated. Check millimetre units, travel limits, speed, temperature, extrusion, and controller arc support before generating output.

## Validation and license

Recorded release checks: 100 offline regressions passed, zero structural errors across 21 PATH components, and registration, new-instance, icon, source, port, and default checks passed for all 26 components. Chinese descriptions survived solving and archive readback. Representative chains passed save/reopen and development-directory isolation checks.

Network upload, physical printing for this version, and installation after an operating-system restart were not tested. These records reflect the release test conditions, not arbitrary equipment or canvas definitions.

This repository contains icons and bilingual documentation. Installation attachments are distributed through GitHub Releases. [MIT License](LICENSE).
