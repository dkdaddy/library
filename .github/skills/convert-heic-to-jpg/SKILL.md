---
name: convert-heic-to-jpg
description: 'Convert HEIC photos to JPG using ImageMagick. Use when: converting photos, HEIC to JPG, batch image conversion, deleting HEIC originals, unzipping and converting photos.'
argument-hint: 'Folder path containing HEIC files (or zip archive)'
---

# Convert HEIC to JPG

## When to Use
- Convert HEIC photos to JPG format
- Batch convert all HEIC files in a folder
- Replace HEIC originals with JPG versions
- Unzip a photo archive, convert, and clean up

## Procedure

1. Identify the target folder containing `.HEIC` files (or a `.zip` archive)
2. If a zip archive is present, extract it and delete the zip:

```powershell
Expand-Archive -Path "<folder_path>\<archive>.zip" -DestinationPath "<folder_path>" -Force
Remove-Item "<folder_path>\<archive>.zip"
```

3. Convert all HEIC files to JPG, deleting originals on success:

```powershell
Get-ChildItem "<folder_path>\*.HEIC" | ForEach-Object { $jpg = $_.FullName -replace '\.HEIC$','.jpg'; magick $_.FullName $jpg; if (Test-Path $jpg) { Remove-Item $_.FullName } }
```

4. Verify the conversion by listing the folder contents

## Notes
- Requires ImageMagick (`magick`) to be installed and on PATH
- Only deletes the original HEIC file if the JPG was successfully created
- Case-sensitive match on `.HEIC` extension — adjust if files use `.heic`
