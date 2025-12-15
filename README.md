## 1. Set up a GitHub repo for your assets

1. Go to GitHub → **New repository**.
2. Name it something like `homebrew-assets`.
3. Make it **Public** (this is important — Homebrewery can’t see private repos).
4. Create it, then add two folders (optional but tidy):

   * `img/` for images
   * `fonts/` for fonts

You can upload via the web UI:

* Go into `img/` → **Add file → Upload files** → drop your PNG/JPG.
* Same for `fonts/` with `.woff2`, `.woff`, `.ttf`, etc.

> Tip: avoid spaces and weird characters in filenames.

---

## 2. Get a direct (“raw”) URL from GitHub

For **each file** (image or font):

1. Click the file in GitHub.
2. Click the **Raw** button.
3. Copy the URL in your browser’s address bar – it’ll look like:

```text
https://raw.githubusercontent.com/YourUser/homebrew-assets/branch/img/my-image.png
```

⚠️ Don’t use the `.../blob/...` URL — it must be the `raw.githubusercontent.com` version (or a `?raw=1` URL) or it won’t work properly.

To convert URL
1. Replace https://github.com/ with https://raw.githubusercontent.com/
2. Remove /blob/
3. Remove ?raw=true

---

## 3. Use GitHub images in Homebrewery

In your Homebrewery brew, just drop the raw URL into normal Markdown image syntax:

```markdown
![My Cool Map](https://raw.githubusercontent.com/YourUser/homebrew-assets/main/img/my-map.png)
```

or with HTML if you want more control:

```html
<img src="https://raw.githubusercontent.com/YourUser/homebrew-assets/main/img/my-map.png" class="wide" />
```

That’s it — Homebrewery will fetch the image from GitHub when the page loads.

---

## 4. Host fonts on GitHub and import them in Homebrewery

### 4.1 Upload your font files

In your `fonts/` folder, upload the font formats you have the rights to use:

* Prefer `.woff2` (best), then `.woff`
* Only use `.ttf` / `.otf` if needed

> ⚠️ Licensing note: only self-host fonts you’re allowed to (open-source fonts or fonts you’ve licensed for web embedding).

Get the **raw URLs** for each file, just like you did with images.

Example:

```text
https://raw.githubusercontent.com/YourUser/homebrew-assets/main/fonts/MyFont-Regular.woff2
https://raw.githubusercontent.com/YourUser/homebrew-assets/main/fonts/MyFont-Regular.woff
```

### 4.2 Add `@font-face` in Homebrewery CSS

In Homebrewery, open the **Styles** tab and add something like:

```css
@font-face {
  font-family: 'MyBookFont';
  src: url('https://raw.githubusercontent.com/YourUser/homebrew-assets/main/fonts/MyFont-Regular.woff2') format('woff2'),
       url('https://raw.githubusercontent.com/YourUser/homebrew-assets/main/fonts/MyFont-Regular.woff') format('woff');
  font-weight: normal;
  font-style: normal;
}

/* Optional: bold/italic faces as separate @font-face blocks */
@font-face {
  font-family: 'MyBookFont';
  src: url('https://raw.githubusercontent.com/YourUser/homebrew-assets/main/fonts/MyFont-Bold.woff2') format('woff2');
  font-weight: bold;
  font-style: normal;
}
```

### 4.3 Apply the font to your brew

Then still in the Styles tab, target the elements you want to change. With the v3 renderer you’re already using, scoping to `.page` is nice and safe:

```css
.page {
  font-family: 'MyBookFont', serif;
}

/* Example: only for headings */
.page h1, 
.page h2, 
.page h3 {
  font-family: 'MyBookFont', serif;
}
```

Homebrewery will load the font from GitHub and apply it to the rendered pages.

---

## 5. Common gotchas

* **Repo must be public** – private repos won’t serve assets to Homebrewery.
* **Use HTTPS** – GitHub raw URLs are HTTPS by default; don’t change that.
* **Cache behavior** – if you replace a file but keep the same filename, browsers may cache the old version. You can:

  * Change the filename, *or*
  * Add a version query, e.g.
    `.../my-map.png?v=2`
* **File size** – keep images reasonable (compressed PNG/JPG/WebP) and fonts minimal (only the weights/styles you need).
