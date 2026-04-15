# STL Support Cutter

Detects overhanging faces in STL files and cuts the model into support-free parts. Entirely client-side — no server, no dependencies to install.

**Live:** https://congruentlabs.github.io/stl-cutter/

## How it works

1. Upload a binary or ASCII STL (up to 200 MB)
2. The app detects faces whose normals exceed the overhang threshold (default 45°)
3. **Auto-orient** (optional): tests all 6 rectangular-prism (bounding-box) face orientations plus up to 60 unique mesh face normals and rotates the model to the orientation with the lowest overhang area — minimising the number of cuts needed before any slicing occurs
4. A greedy interval-cover algorithm places the minimum number of horizontal cut planes needed so that every overhang face is within the cut tolerance of the cut plane below it
5. Each triangle crossing a cut plane is clipped using Sutherland-Hodgman, cut edges are chained into polygon loops and fan-triangulated to close the faces
6. Parts are rendered in Three.js — click parts to isolate them, toggle visibility per-part
7. Download as individual STLs or a ZIP

## Settings

| Setting | Default | Description |
|---|---|---|
| Overhang threshold | 45° | Faces angled beyond this from horizontal are flagged |
| Min face area | 2 mm² | Filters mesh noise and near-horizontal micro-faces |
| Cut tolerance | 1 mm | Maximum distance an overhang may sit above its resolving cut plane; also controls when adjacent overhangs can share a single cut |
| Auto-orient model | OFF | Rotates the model to the orientation that minimises overhang area before cutting; tests 6 rectangular-prism faces plus up to 60 unique mesh face normals |

## Limitations

- Cuts are horizontal only
- Cap triangulation uses centroid-fan, which won't correctly close concave cross-sections (e.g. hollow tubes)
- Internal cavities are not detected (inward-facing normals)
- Auto-orient exports parts in the rotated coordinate system; re-orient in your slicer if needed

## Tech

Three.js r128 · STLLoader · OrbitControls · JSZip 3.10
