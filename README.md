# Asset Source Library

A library of raw source material for 3D reconstruction and photogrammetry assets. Each asset folder contains capture imagery and associated metadata required for processing.

## Contents

| Folder | Description |
|--------|-------------|
| `park_bench_1/` | Park bench — 9 capture images |

## Folder Structure

Each asset is self-contained in its own folder:

```
<asset_name>/
├── images/               # Raw capture photos (.jpg, .png, etc.)
├── gcps.csv              # Ground control points (label, X, Y, Z, image coords)
├── camera_poses.json     # Per-image camera position and orientation
├── point_cloud.ply       # Dense or sparse point cloud
└── metadata.json         # Capture conditions, equipment, notes
```

> Not all assets will have every file type. Include what is available.

## Metadata Standards

### Ground Control Points (`gcps.csv`)

```
label,world_x,world_y,world_z,image_x,image_y,image_filename
GCP_01,123.456,789.012,34.567,1024,768,IMG_0898.jpg
```

Coordinates should be in a defined CRS (e.g., WGS84 / EPSG:4326, or local).

### Camera Poses (`camera_poses.json`)

```json
{
  "IMG_0898.jpg": {
    "position": [x, y, z],
    "rotation": [qw, qx, qy, qz],
    "focal_length_mm": 24.0,
    "sensor_width_mm": 36.0
  }
}
```

### Point Cloud (`*.ply`, `*.las`, `*.laz`)

Preferred formats: PLY, LAS/LAZ. Include coordinate system in filename or metadata where possible.

### Asset Metadata (`metadata.json`)

```json
{
  "asset_name": "park_bench_1",
  "capture_date": "YYYY-MM-DD",
  "location": "Description or coordinates",
  "camera": "Make Model",
  "lens_mm": 24,
  "image_count": 9,
  "crs": "EPSG:4326",
  "notes": ""
}
```

## Naming Conventions

- Asset folders: `snake_case` with a brief descriptive name (e.g., `park_bench_1`, `fire_hydrant_downtown_2`)
- Images: preserve original camera filenames where possible
- Suffixes for variants: `_v2`, `_overhead`, `_close` etc.

## Adding a New Asset

1. Create a new folder under the repo root using the naming convention above.
2. Add capture images.
3. Add any available metadata files following the standards above.
4. Update the Contents table in this README.
