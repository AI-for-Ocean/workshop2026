# Gallery Submission Workflow

## Overview

The Activities page displays a picture gallery. Workshop participants email photos with captions to `wangs.coordinator@gmail.com`. The coordinator (or Hermes inbox-expert) processes the email and adds the photo to the gallery.

## How participants submit

1. Participant emails `wangs.coordinator@gmail.com` with:
   - **Subject**: anything (e.g. "Workshop photo")
   - **Attachment**: one or more photos (jpg, png, etc.)
   - **Body**: a short caption for the photo(s)

2. The submitter's name is identified from the sender's email address (looked up in the participants DB by email).

## How to add a photo to the gallery

### Step 1: Save the image

Save the attached photo to:
```
src/assets/gallery/
```
Use a descriptive filename, e.g. `group-photo-day1.jpg`. Avoid spaces — use hyphens.

### Step 2: Add an import in Activities.astro

At the top of `src/components/Activities.astro`, add an import line:
```js
import groupPhotoDay1 from "../assets/gallery/group-photo-day1.jpg";
```
And add it to the `galleryImages` map:
```js
const galleryImages: Record<string, ImageMetadata> = {
  "sample-workshop-photo.png": sampleWorkshopPhoto,
  "group-photo-day1.jpg": groupPhotoDay1,
};
```

### Step 3: Add an entry to gallery.json

In `src/data/gallery.json`, append:
```json
{
  "image": "group-photo-day1.jpg",
  "caption": "The caption from the email body.",
  "submitter": "Participant Name",
  "date": "2026-07-22"
}
```

- `image`: the exact filename saved in Step 1
- `caption`: the caption from the email (use the email body text)
- `submitter`: the participant's name (looked up by sender email in the DB)
- `date`: the date of the photo (use email date if photo date is unknown)

### Step 4: Build, commit, push

```bash
cd workshop2026/
npm install && npm run build
git add src/assets/gallery/ src/data/gallery.json src/components/Activities.astro
git commit -m "Add gallery photo: [description]"
git push origin web
```

## Data format reference

`gallery.json` is an array of objects, sorted newest-first on the page:

| Field      | Type   | Description                                      |
|------------|--------|--------------------------------------------------|
| image      | string | Filename of the image in `src/assets/gallery/`   |
| caption    | string | Short description of the photo                   |
| submitter  | string | Name of the person who submitted the photo       |
| date       | string | ISO date (YYYY-MM-DD) of when photo was taken    |

## Notes

- The gallery is displayed newest-first (sorted by date descending).
- If an image is not found in the import map, a placeholder (background.png) is shown.
- Astro requires explicit imports for static assets — you cannot reference images by string path alone.
- Square or 4:3 aspect ratio photos work best; the grid crops to 4:3.