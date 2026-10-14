# vision-label-matcher

A small Flask web app (2018) that labels an image with the Google Cloud Vision API
and matches those labels against a list of sentences stored in Firebase.

## How it works

1. Drag an image onto the page (or pick one from your computer).
2. The browser sends it base64-encoded to `POST /predict`.
3. The server asks Cloud Vision for **label detection** on the image.
4. The top 3 labels are fuzzy-matched (`fuzzywuzzy`, score ≥ 50) against the
   `sentences` list in a Firebase Realtime Database.
5. The page shows both lists: the Cloud Vision labels and the matched sentences.

`GET /find/words?words=a,b,c` runs the same fuzzy match on words you pass in.

## Layout

```
app.py                Flask server: /, /predict, /find/words
templates/index.html  upload page
static/js/main.js     drag-and-drop upload + results rendering (jQuery)
static/css/main.css   styles
```

## Running it

Written for Python 3 with the 2018-era client libraries
(`google-cloud-vision` 0.x, which still had `vision.types` and `vision.enums`).

```
pip install flask "google-cloud-vision<0.40" firebase-admin fuzzywuzzy
python app.py
```

It needs two Google Cloud service-account key files next to `app.py`. They are
**not** in this repo, and `.gitignore` keeps them out:

- a key with Cloud Vision access, loaded through `GOOGLE_APPLICATION_CREDENTIALS`
- a Firebase Admin SDK key for the Realtime Database

Update the filenames and the `databaseURL` in `app.py` to point at your own project.

## Status

Unmaintained. Kept for reference.
