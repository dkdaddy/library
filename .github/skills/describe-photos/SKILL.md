---
name: describe-photos
description: 'Describe photos in a library folder by viewing them. Use when: writing image descriptions, creating ai.txt, describing what is in photos, viewing large JPG images.'
argument-hint: 'Folder path containing photos to describe'
---

# Describe Photos

## When to Use
- Write detailed descriptions of photos in a library folder
- Create an `ai.txt` file with image-by-image descriptions
- View large JPG/image files that exceed the context window

## Procedure

1. List the target folder to identify image files
2. Check image dimensions to determine if resizing is needed:

```powershell
magick identify "<folder_path>\<image>.jpg"
```

3. If images are too large to view directly (typically >1–2MB), create temporary thumbnails:

```powershell
cd "<folder_path>"
foreach ($f in @("IMG_0001","IMG_0002")) {
    magick "$f.jpg" -resize 800x800 -quality 60 "${f}_thumb.jpg"
}
```

4. View each thumbnail using the image viewer tool, one at a time
5. Write descriptions into `ai.txt` alongside the original photos, with:
   - A heading identifying the image set
   - An overall summary of the subject
   - A per-image section describing what is visible in each photo
6. Delete the temporary thumbnails:

```powershell
Remove-Item "<folder_path>\*_thumb.jpg"
```

## Notes
- Requires ImageMagick (`magick`) to be installed and on PATH
- 800x800 at quality 60 is usually sufficient for description purposes
- View images one at a time — batch viewing may exceed context limits
- Include material, colour, condition, and spatial details in descriptions
- Reference any existing `description.txt` or `estimates.md` for context
