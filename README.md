# Sendy Errands — website redesign

A static design demo for the Sendy Errands website, built to be dropped into
the existing PHP site once approved.

## Running it

```bash
node build/serve.js        # http://localhost:8790
```

Any static server works — there is no build step for the site itself. (Python's
`http.server` resets connections when the page requests ~20 images at once, so
`build/serve.js` exists to avoid that.)

## What's here

```
public/
  index.html          the homepage
  book.html           the errand request flow
  services/*.html     seven service detail pages (generated)
  styles.css          one stylesheet for every page
  img/                photography, illustrations, service art
build/
  build-services.js   regenerates services/*.html from one data source
  serve.js            local preview server
assets-raw/           source images before background removal (gitignored)
```

### Regenerating the service pages

```bash
node build/build-services.js
```

The seven pages come from one data array, mirroring how the PHP site already
works — `services/market-shopping.php` is a three-line stub that sets a slug and
includes `services/_template.php`. Keeping that shape means the port back is
mechanical: the data becomes the `services` table, the template becomes
`_template.php`.

## Design notes

**Colour.** Pink carries the structural weight — the nav, the primary buttons,
the emphasis. Text stays near-black; pink body copy at paragraph size is
unreadable. Everything is a CSS custom property at the top of `styles.css`.

**Type.** Plus Jakarta Sans throughout, at one family across weights.

**The hero image is hidden below 760px.** Stacked, it pushes the pickup and
drop-off fields — the page's primary action — below the fold, which is a poor
trade for a decorative photo.

**Nothing pretends to work.** The app store buttons are labelled "coming soon"
rather than linking nowhere, and the booking form says plainly that it is a demo
instead of showing a fake confirmation for an errand nobody will run.

## Wiring it to the real site

The booking form's field names match `book.php` exactly — `service`, `pickup`,
`dropoff`, `item_description`, `instructions`, `date`, `time`, `name`, `phone`,
`alt_phone`, `email`. The hero form submits by GET, so pickup and drop-off arrive
as query params, the same shape `book.php` already reads.

`styles.css` drops into `assets/css/`. The header and footer in each page are
marked with comments showing where they map to `includes/header.php` and
`includes/footer.php`.

## Known issues with the generated artwork

- Some step illustrations have headings baked into the image, which can repeat
  the HTML heading beside them.
- The three customer stories are **placeholder copy inherited from the current
  site** — not real reviews. These need replacing before the site goes public.

Fixed: the price illustration was denominated in Kenyan Shillings and now reads
NGN, and the delivery illustration was a drone and is now a bicycle.

## Assets

Background removal was done with remove.bg. `REMOVE_BG_API_KEY` lives in
`.env.local`, which is gitignored and must stay that way.
