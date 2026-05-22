# C_References — Civil 3D managed assemblies

This folder holds the Autodesk Civil 3D / AutoCAD .NET reference DLLs the plugin
compiles against. The binaries are **not** tracked in git: they are vendor files
from your local Civil 3D install and are version-specific.

Before building `Civil3D-MCP-Plugin/Civil3DMcpPlugin.csproj`, copy the five DLLs
listed below from your installed Civil 3D version into this folder.

## Civil 3D 2026 (current default)

| File              | Source path                                                      |
|-------------------|------------------------------------------------------------------|
| `accoremgd.dll`   | `C:\Program Files\Autodesk\AutoCAD 2026\accoremgd.dll`           |
| `AcDbMgd.dll`     | `C:\Program Files\Autodesk\AutoCAD 2026\acdbmgd.dll`             |
| `acmgd.dll`       | `C:\Program Files\Autodesk\AutoCAD 2026\acmgd.dll`               |
| `AecBaseMgd.dll`  | `C:\Program Files\Autodesk\AutoCAD 2026\ACA\AecBaseMgd.dll`      |
| `AeccDbMgd.dll`   | `C:\Program Files\Autodesk\AutoCAD 2026\C3D\AeccDbMgd.dll`       |

## Other Civil 3D versions

The same five DLLs ship with every supported release. Substitute `AutoCAD 2026`
with `AutoCAD 2025`, `AutoCAD 2024`, etc. The csproj references are version-
agnostic — the build picks up whatever DLLs are present here.

## Pointing the build elsewhere

The csproj resolves the folder via the `Civil3DReferencesPath` MSBuild property
(defaults to `..\C_References`). To build against DLLs in a different location
without copying:

```powershell
dotnet build Civil3D-MCP-Plugin/Civil3DMcpPlugin.csproj `
  -p:Civil3DReferencesPath="C:\Program Files\Autodesk\AutoCAD 2026"
```

(Note: `AecBaseMgd.dll` and `AeccDbMgd.dll` live in subfolders of the install,
so the override only works cleanly if you stage all five DLLs in one folder.)

## Why these aren't checked in

- Autodesk's API DLLs are licensed for use against your installed product, not
  for redistribution.
- Each release ships ~20 MB of binary diff — needless repo bloat.
- Version-specific binaries would force everyone to one Civil 3D release.
