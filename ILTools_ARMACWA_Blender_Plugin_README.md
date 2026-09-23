# ILTools ARMA CWA Blender Plugin

Version **1.1.0** for Blender 3.6 and later (including 4.x and 5.2 LTS).

Edit **Operation Flashpoint / Arma: Cold War Assault** models in Blender: import and export unbinarized **SP3X MLOD** `.p3d` files, manage LODs, named selections, memory points, ComponentXX, named properties, and OFP texture paths.

It replaces the day-to-day Objektiv2 Light habits. It does not replace TexView, Pal2PacE, or MakePbo.

---

## Install

1. In Blender: **Edit → Preferences → Add-ons**.
2. Disable or remove any older **Objektiv2 Workflow** add-on.
3. **Install from Disk** → `ILTools_ARMACWA_Blender_Plugin_V1.1.0.zip`.
4. Enable **ILTools ARMA CWA Blender Plugin**.
5. In the 3D View press `N` and open the **Objektiv2** tab.

The zip name includes `V1.1.0`. The folder inside must stay `ILTools_ARMACWA_Blender_Plugin` (Python packages cannot contain dots).

---

## What the plugin can open

| File | Supported |
|---|---|
| OFP / CWA **MLOD** `.p3d` (`MLOD` + `SP3X` header) | Yes — File → Import → **OFP MLOD (.p3d)** |
| Binarized **ODOL** `.p3d` | No — convert with odol2mlod first |
| Arma 3 **P3DM** `.p3d` | No — use Arma 3 Object Builder |
| `.paa` / `.pac` | No — convert with Pal2PacE / TexView 2 for the viewport only |
| `.pbo` | No — unpack with ExtractPbo / Mikero first |

To check a file: open it in a text editor. The first four letters should be `MLOD`. If they are `ODOL`, it is not editable yet.

---

## Import an existing addon model

Example: `Mortar120mm.p3d` from `ILCTI1.01BaseAddon`.

1. Unpack the addon `.pbo` so the `.p3d` and `.paa` files are on disk.
2. File → Import → **OFP MLOD (.p3d)** and pick the model.
3. Outliner: collection `O2L_Mortar120mm` with one mesh per LOD  
   (`Mortar120mm Resolution 0`, `Resolution 1.5`, `Geometry`, `Memory`, …).
4. Delete Blender’s default **Cube** (`X`) if it is still in the scene. It is not part of the model.
5. Work on **Resolution 0** for the visible mesh.

**Model name** in the sidebar only names collections. It does not load a vehicle from a library.

---

## See textures in Blender

The game uses `.paa`. Blender does not. Convert copies to PNG for the viewport. Leave the **material names** as OFP paths.

### Convert with Pal2PacE (recommended)

Pal2PacE lives next to TexView 2, usually:

`C:\Program Files (x86)\Steam\steamapps\common\Arma 3 Tools\TexView2\Pal2PacE.exe`

```bat
cd /d "C:\path\to\ILCTI1.01BaseAddon"
set P2P="C:\Program Files (x86)\Steam\steamapps\common\Arma 3 Tools\TexView2\Pal2PacE.exe"

%P2P% barrel.paa barrel.png
%P2P% mount.paa mount.png
%P2P% ammobox.paa ammobox.png
%P2P% wheel.paa wheel.png
%P2P% flfront.paa flfront.png
%P2P% wside.paa wside.png
%P2P% flside.paa flside.png
%P2P% optic.paa optic.png
```

### Convert with TexView 2

File → **Save As…** (not Save). In the file name, change `barrel.paa` to `barrel.png` or `barrel.tga`. There is no format dropdown; the extension is the format.

### Assign in Blender 5.2

There is no **Use Nodes** checkbox.

1. Select **Mortar120mm Resolution 0**.
2. Properties editor (right) → red sphere → **Material**.
3. Pick `ILCTI1.01BaseAddon\barrel.paa`.
4. Surface should already be **Principled BSDF**.
5. Next to **Base Color**, click the small circle → **Image Texture** → Open `barrel.png`.
6. Repeat for the other materials.
7. 3D View top-right: **Material Preview** (third sphere).

Do **not** rename the material. The name `ILCTI1.01BaseAddon\barrel.paa` is what export writes into the p3d.

Geometry, Memory, Land Contact, and Fire Geometry have no painted textures. Use Resolution 0 to check the look.

---

## Texture paths and the PBO

CWA loads a face path as `\PREFIX\file.paa`.  
`ILCTI1.01BaseAddon\barrel.paa` means:

- PBO **prefix** = `ILCTI1.01BaseAddon`
- File inside that PBO = `barrel.paa`

If that is already the addon’s prefix, **do not remap**. Wrong prefix = white/pink model in game even when Blender looks fine.

If you must change prefix (new addon name):

- Sidebar → Textures → **Remap texture prefix**  
  Old: `ILCTI1.01BaseAddon\`  
  New: `youraddon\`
- Or **Remap textures in .p3d on disk** to rewrite the file without importing.

After remap, the PBO prefix and the folder of `.paa` files must match the new string.

---

## Export back to CWA

1. File → Export → **OFP MLOD (.p3d)**.
2. Put the `.p3d` next to the original `.paa` files.
3. Pack with the same prefix the materials use (`ILCTI1.01BaseAddon` unless you remapped).
4. Test in Cold War Assault.

Export writes **SP3X MLOD**. CWA accepts MLOD in addons. You do not have to binarize.

---

## Start a model from scratch

1. Set **Model name** (for example `hummer`).
2. **Create Objektiv2 model** — builds Resolution, Geometry, Memory, Land Contact, Hit-points, optional View / Shadow / Fire+View Geometry.
3. Model or import an OBJ/FBX into `{name} Resolution 0`. Delete the default Cube.
4. Copy / decimate into Resolution 1, 2, …
5. Geometry: closed convex pieces. In Edit Mode select one piece → **ComponentXX**.
6. Memory: place the 3D cursor, select the Memory mesh → **Add memory point** (`zasleh`, `usti hlavne`, …).
7. **Stamp OFP texture path** or name materials `data\foo.paa` / `youraddon\foo.paa`.
8. **Validate Objektiv2 model**. Full log: text block **Objektiv2 Report**.

---

## Sidebar panels

| Panel | What it does |
|---|---|
| Objektiv2 | Model name, texture root, create LOD set, validate |
| Active LOD | Mark LOD type, resolution, named selections, ComponentXX |
| Memory points | Vertex at the 3D cursor (Memory / Hit-points / Land Contact only) |
| Named properties | `#Property#` keys (`forcenotalpha`, `lodnoshadow`, …) |
| Textures | Stamp path, remap prefix, remap a p3d on disk |

---

## How Objektiv2 maps to Blender

| Objektiv2 Light | This plugin |
|---|---|
| LOD list | One mesh + collection per LOD, `is_lod` on the object |
| Named selections | Vertex groups |
| Memory points | Loose vertices + a vertex group each |
| Component01–99 | Vertex groups, auto-numbered |
| Named properties | Per-LOD key / value list |
| Texture on a face | Material **name** (`ILCTI1.01BaseAddon\barrel.paa`) |
| Pal2Pac | External: Pal2PacE, TexView 2, ImageToPAA |

---

## What it will not do

- Open ODOL or Arma 3 P3DM.
- Decode `.paa` in the viewport (use Pal2PacE → PNG).
- Pack or unpack `.pbo`.
- Binarize to ODOL.
- Licence / KEYLIB checks.

---

## Changelog

### 1.1.0

- File → Import / Export → OFP MLOD (.p3d) (SP3X).
- Remap texture prefix in the scene or on a p3d on disk.

### 1.0.1

- Correct functional LOD floats (Land Contact `2e15` … Fire Geometry `7e15`).
- Collections scoped under `O2L_<model>`.
- Validate scoped to the current model; full report in a text block.
