# Asset Source Library

A library of raw source material for 3D reconstruction and photogrammetry assets. Each asset folder contains capture imagery and associated metadata required for processing.

## Contents

| Folder | Description |
|--------|-------------|
| `park_bench_1/` | Park bench — 9 capture images, GCPs, camera poses, marks |

## Folder Structure

Each asset is self-contained in its own folder. Files are flat (no sub-folders) and co-located with their source images:

```
<asset_name>/
├── gcps.json                 # Asset-level ground control points (world coordinates)
├── IMG_XXXX.jpg              # Capture photo
├── IMG_XXXX.pose.json        # Camera pose solved from GCPs (per image)
└── IMG_XXXX.marks.json       # GCP pixel picks and feature cloud points (per image)
```

> Not all assets will have every file type. Include what is available.

## File Formats

### `gcps.json` — Ground Control Points

One file per asset. Defines named control points in the asset's coordinate space.

```json
{
  "points": [
    { "id": "1", "x": 0.0,    "y": 0.0,   "z": 0.0 },
    { "id": "2", "x": 0.0,    "y": 600.0, "z": 0.0 },
    { "id": "3", "x": 1800.0, "y": 600.0, "z": 0.0 },
    { "id": "4", "x": 1800.0, "y": 0.0,   "z": 0.0 }
  ],
  "crs": "local",
  "units": "mm",
  "version": "1.0"
}
```

| Field | Description |
|-------|-------------|
| `points[].id` | Unique GCP identifier, matches `id` in `.marks.json` |
| `points[].x/y/z` | World-space coordinates |
| `crs` | Coordinate reference system — `"local"` for asset-relative, or an EPSG code |
| `units` | Unit of the world coordinates (`mm`, `m`, etc.) |

---

### `<image>.pose.json` — Camera Pose

One file per image. Stores the solved camera extrinsics, intrinsics, GPS prior, and solve diagnostics.

```json
{
  "source": "IMG_0898.jpg",
  "version": "1.0",
  "extrinsics": {
    "rotation": [
      [-0.865, 0.501, -0.025],
      [ 0.266, 0.417, -0.869],
      [-0.425,-0.759, -0.494]
    ],
    "translation": [509.21, -741.44, 3221.27]
  },
  "intrinsics": {
    "image_width": 5712, "image_height": 4284,
    "cx": 2856.0, "cy": 2142.0,
    "fx": 3960.56, "fy": 3960.56,
    "k1": 0.0, "k2": 0.0, "p1": 0.0, "p2": 0.0
  },
  "gps_prior": {
    "latitude": 51.4479, "longitude": -0.0777,
    "altitude": 40.37, "sigma_m": 5.0
  },
  "solve_meta": {
    "source": "gcp_pnp",
    "converged": true,
    "inlier_count": 4,
    "reprojection_error_px": 46.76
  }
}
```

| Section | Field | Description |
|---------|-------|-------------|
| `extrinsics` | `rotation` | 3×3 rotation matrix (world → camera) |
| `extrinsics` | `translation` | Camera translation vector in world units |
| `intrinsics` | `fx`, `fy` | Focal lengths in pixels |
| `intrinsics` | `cx`, `cy` | Principal point (pixels) |
| `intrinsics` | `k1`,`k2`,`p1`,`p2` | Radial and tangential distortion coefficients |
| `gps_prior` | | WGS84 GPS position used as a prior; `sigma_m` is position uncertainty in metres |
| `solve_meta` | `source` | Method used — `gcp_pnp` = PnP solved from GCP marks |
| `solve_meta` | `reprojection_error_px` | Mean reprojection error in pixels |

---

### `<image>.marks.json` — GCP Marks and Feature Points

One file per image. Records the pixel locations of GCP picks and detected cloud feature points.

```json
{
  "source": "IMG_0898.jpg",
  "version": "1.0",
  "imageWidth": 5712,
  "imageHeight": 4284,
  "camera": {
    "x": 2007.0, "y": 2497.6, "z": 958.9,
    "tgtX": 1005.8, "tgtY": 760.7, "tgtZ": -164.7
  },
  "gcpMarks": [
    { "id": "1", "u": 3659.25, "v": 1199.52 },
    { "id": "2", "u": 4012.68, "v": 1438.71 },
    { "id": "3", "u": 1378.02, "v": 2117.01 },
    { "id": "4", "u": 1163.82, "v": 1720.74 }
  ],
  "cloudPoints": [
    { "id": 0, "x": 1712.17, "y": 1631.49 },
    { "id": 1, "x": 2543.27, "y": 1430.14 }
  ]
}
```

| Field | Description |
|-------|-------------|
| `camera.x/y/z` | Camera position in world space (derived from pose) |
| `camera.tgtX/Y/Z` | Camera look-at target in world space |
| `gcpMarks[].id` | Matches `points[].id` in `gcps.json` |
| `gcpMarks[].u/v` | Pixel column/row of the GCP pick in the image |
| `cloudPoints[].id` | Index into the feature cloud |
| `cloudPoints[].x/y` | Pixel column/row of the feature point |

## Naming Conventions

- Asset folders: `snake_case` with a brief descriptive name (e.g., `park_bench_1`, `fire_hydrant_downtown_2`)
- Images: preserve original camera filenames where possible
- Suffixes for variants: `_v2`, `_overhead`, `_close` etc.

## Adding a New Asset

1. Create a new folder under the repo root using the naming convention above.
2. Add capture images.
3. Add any available metadata files following the standards above.
4. Update the Contents table in this README.
