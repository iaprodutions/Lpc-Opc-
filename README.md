## Latest update
- The subtopic manager now appears inside the Study slide settings area alongside Add slide, Delete slide, Save and Choose images.
- Press the gear again to close the settings controls and hide the subtopic manager. If there are unsaved slide edits, the app asks before discarding them.

# LPC & OPC Study Companion — Local Storage Edition

## What's changed in this update
- Replaced the hidden, programmatically triggered image pickers with visible native iPhone file-selection controls.
- Removed the folder-only (`webkitdirectory`) restriction from image reconnection. You can now choose individual images or multiple images from the iPhone Files picker.
- The slide editor also uses a visible native image picker for inserting images directly into a slide.
- Updated the service-worker cache version so GitHub Pages clients receive the new picker UI.
- Removed an outdated Supabase message from the local-storage error alert.

## Local storage features
- Supabase is not used.
- Images inserted into slides are copied into the app's local IndexedDB storage.
- Study text, topics, subtopics, notes and review cards continue to be saved locally.
- Settings includes a study-content JSON backup (images intentionally excluded), restore, and image import from Files.
- Image import matches selected files to saved slide image placeholders using the original image filename (extension ignored).
- Requests persistent browser storage where supported and displays browser storage usage/quota estimates when available.

## Important iPhone notes
- iOS controls website storage quotas and may clear website data. There is no guaranteed unlimited quota. Keep regular JSON backups and keep the original `LPCOPCfiles` image folder separately.
- The web app cannot force iOS Files to open only one folder or guarantee permanent folder access. Use the visible file-selection control to choose one or more image files. Folder-only selection is not required.
- Images are excluded from JSON backups. After restoring a backup, import the matching original images again. The image's original filename must be retained for automatic matching.
- Existing images that only existed in Supabase cannot be downloaded after removing Supabase. Before switching, keep the previous app available and retrieve any cloud-only images you still need. Existing image references are preserved as missing placeholders and can be reconnected by importing files with matching original filenames.

## Deploy
Replace `index.html`, `sw.js`, and `manifest.webmanifest` in the GitHub Pages repository. No SQL file or Supabase setup is required.
