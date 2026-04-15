# STL Support Cutter

Detects overhanging faces in STL files and cuts the model into support-free parts. Entirely client-side — no server, no dependencies to install.

**Live:** https://congruentlabs.github.io/stl-cutter/

## How it works

1. Upload a binary or ASCII STL (up to 200 MB)
2. The app detects faces whose normals exceed the overhang threshold (default 45°)
3. Horizontal cut planes are placed at the base of each overhang cluster
4. Each triangle crossing a cut plane is clipped using Sutherland-Hodgman, cut edges are chained into polygon loops and fan-triangulated to close the faces
5. Parts are rendered in Three.js — click parts to isolate them, toggle visibility per-part
6. Download as individual STLs or a ZIP

## Settings

| Setting | Default | Description |
|---|---|---|
| Overhang threshold | 45° | Faces angled beyond this from horizontal are flagged |
| Min face area | 2 mm² | Filters mesh noise and near-horizontal micro-faces |
| Cluster gap | 1 mm | Merges cut planes within this Z distance into one |

## Limitations

- Cuts are horizontal only
- Cap triangulation uses centroid-fan, which won't correctly close concave cross-sections (e.g. hollow tubes)
- Internal cavities are not detected (inward-facing normals)

## Tech

Three.js r128 · STLLoader · OrbitControls · JSZip 3.10
