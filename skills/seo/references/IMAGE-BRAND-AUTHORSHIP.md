# Image brand, authorship, and SEO

Prioritized implementation guide for marketing and development teams. **Only include recommendations documented by [Google image SEO](https://developers.google.com/search/docs/appearance/google-images) or [Google image metadata](https://developers.google.com/search/docs/appearance/structured-data/image-license-metadata).**

## Agent rules (mandatory)

- Do **not** recommend image SEO tactics unless they appear in the sources above, [Schema.org ImageObject](https://schema.org/ImageObject), or [Organization structured data](https://developers.google.com/search/docs/appearance/structured-data/organization) where relevant to site-wide brand.
- Do **not** claim that `llms.txt`, blocking/allowing AI crawlers, or embedded tricks associate images with a brand in Google Search — there is no confirmed Google documentation for those as image-ranking signals.
- Do **not** invent Schema properties or required fields; use Google's documented required/recommended lists for image license metadata.
- **LLMs:** Google does not publish LLM-specific image indexing rules. Context that helps Google (page text, `alt`, JSON-LD) may help other systems parse pages, but that is inference — state it as such or omit.

## Impact priority (high and medium only)

| Priority | Impact | Action | Why (official source) |
|----------|--------|--------|------------------------|
| 1 | **High** | Publish each strategic image on a **dedicated, indexable landing page** with unique `title`, meta description, and copy about the subject | Google: content and metadata of pages where an image is embedded greatly influence how/where the image appears; place images near relevant text on relevant pages |
| 2 | **High** | Embed with **`<img src="..." alt="...">`** (not CSS-only backgrounds for indexable images) | Google indexes `src` on `img`; does not index CSS images |
| 3 | **High** | Write **accurate `alt` text** and a **short descriptive filename** | Google: `alt` is the most important attribute for image metadata; filename gives light subject clues |
| 4 | **High** | Add **`ImageObject` JSON-LD** per image **on every page** where that image appears, with `contentUrl` plus at least one of: `creator`, `creditText`, `copyrightNotice`, `license` | Google image metadata program; repeat markup per page instance |
| 5 | **High** | Mirror license fields in **visible HTML** (links + credit lines) on the same page | Google’s official examples pair JSON-LD with visible license/creator/copyright/credit |
| 6 | **Medium** | Set **`primaryImageOfPage`** (or `image` on the main entity) via JSON-LD and/or **`og:image`** for the preferred preview | Google documents both as ways to influence which image is selected for previews |
| 7 | **Medium** | Submit an **image sitemap** (Yoast/Rank Math can include image tags) | Google: helps discover URLs not found otherwise |
| 8 | **Medium** | Keep **site-wide `Organization`** (name, `url`, `logo`, `sameAs`) accurate and consistent | Google Organization structured data supports brand understanding (not a per-image substitute) |
| 9 | **Medium** | Use **`Product` / `Article` / other valid types** with required `image` when the page is a product or article | Google: structured data types can earn rich results in Google Images when eligible |
| 10 | **Medium** | Embed **IPTC rights metadata once per file** OR JSON-LD per page (either is sufficient for Google’s image metadata features) | Google documents both; IPTC travels with the file, JSON-LD is per page |
| 11 | **Medium** | **Compress**, use modern formats (WebP/AVIF), set `width`/`height`, keep quality high | Google image SEO: speed and quality affect experience and engagement |

**Out of scope for this guide (low/unproven):** `llms.txt`, legal copyright registration alone, C2PA unless you already publish signed manifests, AI crawler rules for “brand SEO”.

---

## 1. Landing page (high)

### What to do

1. Create or improve a page URL that matches the topic (e.g. `/certificado-diamantes/`), not only `/wp-content/uploads/...png`.
2. Use one clear `<h1>`, body copy that names the brand and subject, and internal links from related pages with descriptive anchors.
3. Ensure the page is indexable (no `noindex`, not blocked in `robots.txt`).

### Why

Google associates images with the **page context**, not isolated media URLs.

---

## 2. HTML `img` + alt + filename (high)

### What to do

1. Replace decorative CSS-only heroes used as primary content images with `<img>` when the image should be found in Search.
2. Set `alt` to describe the image in the page context (no keyword stuffing).
3. Rename files before upload: short, descriptive, hyphenated (e.g. `certificado-diamantes-marca.webp`). Avoid `IMG_0001.jpg`.

### Final markup pattern

```html
<img
  src="https://www.example.com/wp-content/uploads/2025/10/certificado-diamantes-marca.webp"
  alt="Certificado de diamantes de Example Store, taller en Barcelona"
  width="1200"
  height="848"
  loading="lazy"
  decoding="async"
/>
```

---

## 3. ImageObject JSON-LD + visible credits (high)

### What to do

1. Add one `ImageObject` block per image in `<head>` or before `</body>`.
2. Use the **same URL** in `contentUrl` as in `img src` (for `srcset`, use the same URL as in Google's `src` fallback — see Google’s srcset example).
3. Include **`contentUrl`** and **at least one** of: `creator`, `creditText`, `copyrightNotice`, `license`.
4. For the **Licensable badge**, include **`license`** (URL to terms). **`acquireLicensePage`** is recommended.
5. Repeat this block on **each page** that shows the image (even if the file is identical).
6. Below the image, add visible lines matching the schema (Google’s examples).

### Required vs recommended (Google)

- **Required:** `contentUrl`; and at least one of `creator` | `creditText` | `copyrightNotice` | `license`
- **Recommended:** `acquireLicensePage`, `creator`, `creator.name`, `creditText`, `copyrightNotice`, `license`

`creator` may be a **Person** or **Organization** (Google: “may be a company or organization if appropriate”).

### Final page example (single image)

Replace URLs and names with real values. Validate at [Rich Results Test](https://search.google.com/test/rich-results).

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>Certificado de diamantes | Example Store</title>
  <meta name="description" content="Certificación de diamantes de Example Store. Joyería en Barcelona.">
  <link rel="canonical" href="https://www.example.com/certificado-diamantes/">
  <meta property="og:image" content="https://www.example.com/wp-content/uploads/2025/10/certificado-diamantes-marca.webp">
  <meta property="og:image:alt" content="Certificado de diamantes Example Store">
  <script type="application/ld+json">
  {
    "@context": "https://schema.org/",
    "@type": "ImageObject",
    "contentUrl": "https://www.example.com/wp-content/uploads/2025/10/certificado-diamantes-marca.webp",
    "license": "https://www.example.com/condiciones-de-uso/",
    "acquireLicensePage": "https://www.example.com/contacto/",
    "creditText": "Example Store — example.com",
    "creator": {
      "@type": "Organization",
      "name": "Example Store",
      "url": "https://www.example.com/"
    },
    "copyrightNotice": "© Example Store. Todos los derechos reservados."
  }
  </script>
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "WebPage",
    "url": "https://www.example.com/certificado-diamantes/",
    "name": "Certificado de diamantes | Example Store",
    "primaryImageOfPage": "https://www.example.com/wp-content/uploads/2025/10/certificado-diamantes-marca.webp",
    "isPartOf": {
      "@type": "WebSite",
      "url": "https://www.example.com/"
    }
  }
  </script>
</head>
<body>
  <main>
    <h1>Certificado de diamantes</h1>
    <p>En Example Store emitimos este certificado para cada diamante…</p>

    <figure>
      <img
        src="https://www.example.com/wp-content/uploads/2025/10/certificado-diamantes-marca.webp"
        alt="Certificado de diamantes de Example Store, joyería en Barcelona"
        width="1200"
        height="848"
        loading="lazy"
        decoding="async"
      />
      <figcaption>Certificado de diamantes — Example Store</figcaption>
    </figure>

    <p><a href="https://www.example.com/condiciones-de-uso/">Licencia de uso</a></p>
    <p><a href="https://www.example.com/contacto/">Cómo solicitar uso de imágenes</a></p>
    <p><strong>Creador</strong>: Example Store</p>
    <p><strong>Copyright</strong>: © Example Store. Todos los derechos reservados.</p>
    <p><strong>Crédito</strong>: Example Store — example.com</p>
  </main>
</body>
</html>
```

### WordPress (no invented hooks)

Reliable options documented in WordPress itself:

1. **Custom HTML block** on the page: paste the `<script type="application/ld+json">` block(s) and the visible credit `<p>` lines.
2. **Child theme template** for that page template only.
3. **Media library:** fill Title, Alt Text, Caption, and Description — aligns with on-page `alt` and captions Google reads from page content.

Do not duplicate conflicting `ImageObject` graphs from SEO plugins; test with Rich Results Test after changes.

---

## 4. primaryImageOfPage + og:image (medium)

Already shown in the example above. Use a **representative** image for the page topic; Google says to avoid generic logos or extreme aspect ratios for preferred-image markup.

---

## 5. Image sitemap (medium)

### What to do

1. Enable XML sitemaps in your SEO plugin.
2. Confirm image entries (`<image:image>`) appear for URLs that embed images.
3. Submit sitemap in Google Search Console.

### Why

Google documents image sitemaps for discovery.

---

## 6. Organization (medium, site-wide)

### What to do

Maintain one correct `Organization` (or more specific subtype, e.g. `Store`) with `name`, `url`, `logo`, `sameAs`. Fix NAP inconsistencies.

### Why

Supports brand entity clarity; does not replace per-image `ImageObject` on content pages.

---

## 7. IPTC in the file (medium, alternative to JSON-LD)

### What to do

1. Once per master file, set IPTC fields Google reads: **Creator**, **Credit line**, **Copyright notice**, **Web statement of rights** (license URL), **Licensor URL** (acquire license page) per [Google IPTC table](https://developers.google.com/search/docs/appearance/structured-data/image-license-metadata#iptc-photo-metadata).
2. Google recommends retaining rights metadata when optimizing file size.

### Why

Google accepts IPTC **or** structured data; IPTC is embedded once and follows the file across pages.

---

## 8. Performance and format (medium)

### What to do

1. Serve WebP or AVIF with a fallback `src` on `img`.
2. Match file extension to actual format.
3. Compress without destroying quality; specify `width` and `height`.

### Why

Documented under image SEO best practices (speed and quality).

---

## Validation checklist

- [ ] Page is indexable and uses `<img src>` with descriptive `alt`
- [ ] `ImageObject` `contentUrl` matches `img src`
- [ ] At least one of creator / creditText / copyrightNotice / license is set
- [ ] Visible license/credit lines exist on the page
- [ ] Rich Results Test passes without errors
- [ ] Image URLs appear in sitemap (if using sitemaps)
- [ ] Search Console → Performance → Search results type **Image** (monitoring, not a ranking lever)

## Sources

- [Google image SEO best practices](https://developers.google.com/search/docs/appearance/google-images)
- [Image metadata in Google Images](https://developers.google.com/search/docs/appearance/structured-data/image-license-metadata)
- [Organization structured data](https://developers.google.com/search/docs/appearance/structured-data/organization)
