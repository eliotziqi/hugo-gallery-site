## Usage & Future Extension

This site is built with Hugo and the `gallerydeluxe` module.

This section documents the intended workflow for:
- Adding new images
- Managing EXIF information
- Creating additional pages or galleries

---

## Adding or Updating Photos

### Where images live

All gallery images are stored under:
```content/images/```


Each image file placed in this directory will automatically be picked up by the gallery on build.

### Adding new photos

1. Copy image files (`.jpg`, `.jpeg`, `.png`) into `content/images/`
2. Run the development server:

```bash
hugo server -D -b http://localhost:1313/
```

3. The gallery will update automatically.

### Replacing existing photos
- Replace the file with the same filename to keep URLs stable.
- Or remove the old file and add a new one — the gallery will re-generate its image data automatically.
No manual index or list update is required.

## EXIF Information

### How EXIF data works

EXIF metadata is read directly from the image files at build time.
This includes fields such as:

* Date
* Lens model
* Exposure time
* Aperture (F number)
* Artist (if present)

### Modifying EXIF data (recommended workflow)

EXIF data should **not** be edited in Hugo templates or JSON output.

Instead, modify EXIF metadata at the image level using external tools:

* `exiftool`
* Lightroom
* Capture One
* Darktable

Example using `exiftool`:

```bash
exiftool -Artist="Your Name" image.jpg
```

After modifying EXIF data:

1. Save the image
2. Restart or reload `hugo server`
3. The updated EXIF info will appear automatically

### Filtering EXIF fields

Displayed EXIF fields are controlled in `config.toml`:

```toml
[imaging.exif]
includeFields = 'Artist|LensModel|FNumber|ExposureTime'
```

Adjust this list to show or hide specific EXIF fields globally.

---

## Creating Additional Pages

### Simple content pages

To add a new static page (e.g. About, Info, Contact):

```bash
hugo new content about.md
```

This will create:

```
content/about.md
```

Edit the front matter and content as needed.
Hugo will automatically generate `/about/`.

### Custom layouts (optional)

If a page requires a custom layout:

1. Create a template:

```
layouts/about.html
```

2. Reference it in the page front matter:

```yaml
layout: about
```

---

## Creating Multiple Galleries (Future Use)

The current setup supports a single gallery sourced from `content/images/`.

To support multiple galleries in the future, a recommended approach is:

```
content/
  galleries/
    cars/
    portraits/
    travel/
```

Each gallery can be rendered using a dedicated page template
and a different `sourcePath`.

This approach keeps galleries isolated and scalable.

---

## Philosophy

* Images are the source of truth.
* EXIF data lives with the image, not the site.
* Hugo templates should remain simple and declarative.
* Customization should favor configuration over code changes.