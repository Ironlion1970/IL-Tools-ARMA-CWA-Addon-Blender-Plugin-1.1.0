# IL Tools Arma CWA Blender Plugin 1.1.0

Edit Operation Flashpoint / Arma: Cold War Assault models in Blender.

**File:** `IL-Tools-ARMA-CWA-Blender-Plugin-1.1.0.zip`  
**Version:** 1.1.0  
**Blender:** 3.6 and later (4.x and 5.2 LTS)  
**Project:** https://github.com/Ironlion1970/IL-Tools-ARMA-CWA-Addon-Blender-Plugin

Import and export unbinarized **SP3X MLOD** `.p3d` files. Manage LODs, named selections, memory points, ComponentXX, named properties, and OFP texture paths.

It replaces day-to-day Objektiv2 Light work. It does not replace TexView, Pal2PacE, or a PBO packer.

Sister tool for packing: [IL Tools PBO Utility](https://github.com/Ironlion1970/IL-Tools-PBO-Utility/releases)

---

## Install

1. Blender → **Edit → Preferences → Add-ons**
2. Disable or remove any older **Objektiv2 Workflow** add-on
3. **Install from Disk** → `IL-Tools-ARMA-CWA-Blender-Plugin-1.1.0.zip`
4. Enable **ILTools ARMA CWA Blender Plugin**
5. In the 3D View press `N` and open the **Objektiv2** tab

The folder inside the zip must stay `ILTools_ARMACWA_Blender_Plugin` (no dots in the package name).

---

## What it can open

| File | Supported |
|---|---|
| OFP / CWA **MLOD** `.p3d` (`MLOD` + `SP3X`) | Yes — File → Import → **OFP MLOD (.p3d)** |
| Binarized **ODOL** `.p3d` | No — convert with odol2mlod first |
| Arma 3 **P3DM** `.p3d` | No — use Arma 3 Object Builder |
| `.paa` / `.pac` | No — convert with Pal2PacE / TexView 2 for the viewport |
| `.pbo` | No — unpack with [IL Tools PBO Utility](https://github.com/Ironlion1970/IL-Tools-PBO-Utility/releases) |

To check a `.p3d`: the first four letters should be `MLOD`.

---

## Export back to CWA / Remastered

1. File → Export → **OFP MLOD (.p3d)**
2. Put the `.p3d` next to the original `.paa` files
3. Pack the addon folder with **IL Tools PBO Utility**
   - Prefix blank
   - Cprs off
   - Product `OFP: Resistance`
4. Copy the `.pbo` into `Addons` or `@Mod\Addons`
5. Test in Cold War Assault or Remastered

Export writes **SP3X MLOD**. The game accepts MLOD in addons. You do not have to binarize.

---

## Start a model from scratch

1. Set **Model name** (for example `hummer`)
2. **Create Objektiv2 model** — Resolution, Geometry, Memory, Land Contact, Hit-points, optional View / Shadow / Fire Geometry
3. Model or import an OBJ/FBX into `{name} Resolution 0`. Delete the default Cube
4. Copy / decimate into Resolution 1, 2, …
5. Geometry: closed convex pieces. Edit Mode → select one piece → **ComponentXX**
6. Memory: 3D cursor → select Memory mesh → **Add memory point**
7. **Stamp OFP texture path** or name materials `data\foo.paa`
8. **Validate Objektiv2 model** — full log in text block **Objektiv2 Report**

---

## Sidebar panels

| Panel | What it does |
|---|---|
| Objektiv2 | Model name, texture root, create LOD set, validate |
| Active LOD | LOD type, resolution, named selections, ComponentXX |
| Memory points | Vertex at the 3D cursor |
| Named properties | `#Property#` keys |
| Textures | Stamp path, remap prefix, remap a p3d on disk |

---

## Objektiv2 → Blender

| Objektiv2 Light | This plugin |
|---|---|
| LOD list | One mesh + collection per LOD |
| Named selections | Vertex groups |
| Memory points | Loose vertices + a vertex group each |
| Component01–99 | Vertex groups |
| Named properties | Per-LOD key / value list |
| Texture on a face | Material name (`addon\barrel.paa`) |

---

## What it will not do

- Open ODOL or Arma 3 P3DM
- Decode `.paa` in the viewport
- Pack or unpack `.pbo` (use PBO Utility)
- Binarize to ODOL

---

## Changelog

### 1.1.0

- File → Import / Export → OFP MLOD (.p3d) (SP3X)
- Remap texture prefix in the scene or on a p3d on disk

### 1.0.1

- Correct functional LOD floats
- Collections scoped under `O2L_<model>`
- Validate scoped to the current model

---

## Support

- Bugs: https://github.com/Ironlion1970/IL-Tools-ARMA-CWA-Addon-Blender-Plugin/issues
- Questions and comments: https://github.com/Ironlion1970/IL-Tools-ARMA-CWA-Addon-Blender-Plugin/discussions

## Source

https://github.com/Ironlion1970/IL-Tools-ARMA-CWA-Addon-Blender-Plugin
