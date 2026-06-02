# Trips Collection — Design

## Goal

Add a new section to rahulsrivathsa.com for writing up personal trips. First entry is Slovenia (June 2025). The section should be a proper Jekyll collection so future trips can be added as easily as new Thoughts or Projects.

## Decisions

| Question | Choice |
|---|---|
| One-off or series? | Series — build as a collection |
| Section name | "Trips" |
| URL prefix | `/trips/:name/` |
| Page content style | Photo-heavy with captions |
| Page layout | Single column, long-form: photos and prose alternate; italic caption beneath each photo |
| Caption alignment | Left |
| Photo border | Subtle 1px border |
| Homepage treatment | Like Thoughts — dated list, newest first |
| Homepage placement | Between Thoughts and Interests |

## Architecture

The site already has two Jekyll collections (`_projects`, `_thoughts`) registered in `_config.yml` and rendered through `_layouts/post.html`. Trips follows the same pattern with one addition: a small Liquid include for the photo-and-caption pairs that recur throughout each post.

### Components

**Collection** — `_trips/`, registered in `_config.yml` with `output: true` and `permalink: /trips/:name/`. Files are markdown with YAML frontmatter.

**Frontmatter shape** — each entry needs:
```yaml
---
layout: post
title: "Slovenia"
date: 2025-06-15
description: "One week through Ljubljana, Lake Bled, and the Soča Valley."
location: "Slovenia"
---
```
`date` drives homepage sort order and the "(Month Year)" label. `description` is rendered once at the top of the post by the existing `post.html` layout. `location` is included for potential future use (e.g., filtering by region); it is not surfaced anywhere in V1.

**Layout** — reuse `_layouts/post.html` as-is. It already produces the back-link, title, date, description block, and content area used by Thoughts. No new layout file.

**Photo include** — `_includes/photo.html`:
```liquid
<figure class="trip-photo">
  <img src="/images/trips/{{ page.slug }}/{{ include.src }}" alt="{{ include.caption | escape }}">
  <figcaption>{{ include.caption }}</figcaption>
</figure>
```
The post's slug (e.g., `slovenia`) auto-prefixes the image path so each post only writes the filename. Authors invoke it as:
```
{% include photo.html src="bled-sunrise.jpg" caption="Lake Bled at sunrise." %}
```

**Styling** — extend the existing `<style>` block in `_layouts/default.html`:
- `figure.trip-photo` — full width of the content column, vertical margin for breathing room, no horizontal padding
- `figure.trip-photo img` — `width: 100%; height: auto; display: block;` with a subtle 1px border in a neutral grey (matching the muted palette already in use)
- `figure.trip-photo figcaption` — italic, smaller than body text, left-aligned, muted grey (consistent with `.blog-date` and `.last-updated`)

**Homepage integration** — add a "Trips" section to `index.md`, placed between the Thoughts and Interests sections. Markup mirrors the Thoughts block:
```liquid
{% assign trips = site.trips | sort: 'date' | reverse %}
{% if trips.size > 0 %}
## Trips

{% for trip in trips %}
- [{{ trip.title }}]({{ trip.url }}) ({{ trip.date | date: "%B %Y" }})
{% endfor %}
{% endif %}
```

**Photo storage** — `images/trips/<slug>/`. The existing `images/` directory has a single flat file (`borrough-team.jpg`); the trips subtree adopts a per-trip folder convention so photos don't pile up at the top level.

## Authoring flow (what Rahul does for each new trip)

1. Create `_trips/<slug>.md` with the frontmatter block above.
2. Create `images/trips/<slug>/` and drop web-sized JPEGs in (target ~1600px wide max).
3. Write prose paragraphs in markdown; insert `{% include photo.html src="..." caption="..." %}` between paragraphs wherever a photo belongs.
4. `git commit && git push`. Cloudflare Pages rebuilds the site.

## V1 scope

In:
- New `_trips/` collection registered in `_config.yml`
- `_trips/slovenia.md` — frontmatter plus a short skeleton (intro paragraph and two example include lines) for Rahul to fill in
- `_includes/photo.html`
- CSS additions to `_layouts/default.html` for `figure.trip-photo`, its image, and its caption
- "Trips" section in `index.md` placed between Thoughts and Interests
- `images/trips/slovenia/` directory (created so the path exists; photos added later by Rahul)

Out:
- No image optimization pipeline. Cloudflare Pages serves the files as-is. If photo weight becomes a problem, revisit later.
- No homepage thumbnail/cover image — the chosen treatment is a plain dated list.
- No lightbox, lazy-loading attribute, or responsive `srcset`. Can be added later if individual trip pages get heavy.
- No changes to the Projects or Thoughts collections or their layouts.

## Testing

- Run `bundle exec jekyll serve` locally and confirm:
  - `/trips/slovenia/` renders with title, date, description block, and the two placeholder photo blocks visible (broken-image icons are expected until photos are added)
  - Homepage shows the new "Trips" section between Thoughts and Interests, with a single dated link to Slovenia
  - Existing Projects and Thoughts pages are unchanged
- After photos are added to `images/trips/slovenia/`, reload `/trips/slovenia/` and confirm photos render full-width with left-aligned italic captions underneath and a subtle border.

## Risks and follow-ups

- **Image weight**: photos committed to git make the repo grow over time. Acceptable for V1 (one trip, a handful of photos). If trips grow, options are: an external image host, Git LFS, or a build-step optimizer. Flag and revisit at the second or third trip.
- **Caption duplication in `alt`**: the include uses the caption as the `alt` attribute. Adequate for accessibility now, but real `alt` text describing the image content (not the caption) is better practice if Rahul ever wants to refine.
