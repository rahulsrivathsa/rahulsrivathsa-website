# Trips Collection Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a "Trips" section to rahulsrivathsa.com as a Jekyll collection, with a first entry skeleton for Slovenia. Photos and prose alternate in a single column; trip page surfaces on the homepage as a dated link between Thoughts and Interests.

**Architecture:** New Jekyll collection `_trips` mirroring the existing `_projects` and `_thoughts` collections. Reuses `_layouts/post.html` unchanged. A small Liquid include (`_includes/photo.html`) renders each photo as a `<figure>` with a `<figcaption>`, auto-prefixing the image path with `/images/trips/<slug>/`. CSS for the figure is added to the inline `<style>` block in `_layouts/default.html`.

**Tech Stack:** Jekyll 3.x (built by GitHub Pages legacy build *and* Cloudflare Pages — the latter serves `rahulsrivathsa.com`), kramdown, jekyll-feed plugin, Ruby/Bundler for local development.

**Verification model:** This is a static site with no test framework. "Tests" mean: run `bundle exec jekyll build`, expect zero errors, then `grep` the generated files in `_site/` for expected output. Each task ends with build + grep + commit.

---

## File Structure

| Path | Action | Responsibility |
|---|---|---|
| `_config.yml` | Modify | Register the `trips` collection |
| `images/trips/slovenia/.gitkeep` | Create | Allow git to track the (initially empty) photo directory |
| `_includes/photo.html` | Create | Liquid template for `<figure>` + `<img>` + `<figcaption>`, with auto-prefixed path |
| `_trips/slovenia.md` | Create | First trip post — frontmatter + skeleton placeholder content |
| `_layouts/default.html` | Modify | Append CSS for `figure.trip-photo`, its `img`, and its `figcaption` |
| `index.md` | Modify | Insert new "Trips" section between Thoughts and Interests |

Files are touched in dependency order: the collection must exist before its documents are processed, and the include must exist before any document references it.

---

## Preflight

- [ ] **Confirm Ruby/Bundler are available and dependencies install**

Run from the repo root:
```bash
bundle install
```

Expected: completes without error. If `Gemfile.lock` was missing, it is now created (and is already gitignored / not tracked — leave it).

If `bundle install` fails because Bundler isn't installed: `gem install bundler` then retry. If Ruby itself is missing, install via `rbenv` or `asdf` per the user's preferred toolchain before continuing.

---

### Task 1: Register the trips collection and create the photo directory

**Files:**
- Modify: `_config.yml` (collections block, around line 15)
- Create: `images/trips/slovenia/.gitkeep`

- [ ] **Step 1: Verify the current collections block**

Run:
```bash
sed -n '14,22p' _config.yml
```

Expected output:
```
# Collections
collections:
  thoughts:
    output: true
    permalink: /thoughts/:name/
  projects:
    output: true
    permalink: /projects/:name/
```

- [ ] **Step 2: Add the `trips` collection to `_config.yml`**

Edit `_config.yml`. Append a third entry under `collections:` so the block reads:

```yaml
# Collections
collections:
  thoughts:
    output: true
    permalink: /thoughts/:name/
  projects:
    output: true
    permalink: /projects/:name/
  trips:
    output: true
    permalink: /trips/:name/
```

- [ ] **Step 3: Create the photo directory with a .gitkeep**

Run:
```bash
mkdir -p images/trips/slovenia
touch images/trips/slovenia/.gitkeep
```

(Git does not track empty directories — the `.gitkeep` is a conventional zero-byte placeholder so the path exists in the repo.)

- [ ] **Step 4: Build the site and verify it succeeds**

Run:
```bash
bundle exec jekyll build
```

Expected: completes without error. Output ends with something like `done in X.XXX seconds.`. No mentions of warnings about unknown collections.

- [ ] **Step 5: Verify the build noticed the new collection**

The `_trips/` directory does not exist yet, so no trip pages should be generated — but the collection should be registered. Run:
```bash
ls _site/trips 2>/dev/null || echo "no trips output yet (expected)"
```

Expected: `no trips output yet (expected)` (the collection is registered, but there are no documents to render).

- [ ] **Step 6: Commit**

```bash
git add _config.yml images/trips/slovenia/.gitkeep
git commit -m "Register trips collection"
```

---

### Task 2: Add the photo include and create the Slovenia skeleton

**Files:**
- Create: `_includes/photo.html`
- Create: `_trips/slovenia.md`

- [ ] **Step 1: Check if `_includes/` exists**

Run:
```bash
ls _includes 2>/dev/null || echo "directory does not exist yet"
```

If the output is `directory does not exist yet`, create it:
```bash
mkdir _includes
```

If it already exists, leave it.

- [ ] **Step 2: Create `_includes/photo.html`**

Write exactly:
```liquid
<figure class="trip-photo">
  <img src="/images/trips/{{ page.slug }}/{{ include.src }}" alt="{{ include.caption | escape }}">
  <figcaption>{{ include.caption }}</figcaption>
</figure>
```

(`page.slug` is auto-derived by Jekyll from the document's filename — for `_trips/slovenia.md`, it resolves to `slovenia`.)

- [ ] **Step 3: Create `_trips/slovenia.md`**

Write exactly:
```markdown
---
layout: post
title: "Slovenia"
date: 2025-06-15
description: "One week through Ljubljana, Lake Bled, and the Soča Valley."
location: "Slovenia"
---

[Replace with your intro paragraph — set the scene for the trip.]

{% include photo.html src="placeholder-1.jpg" caption="Replace with a caption." %}

[Replace with the paragraph that comes between photos.]

{% include photo.html src="placeholder-2.jpg" caption="Replace with a caption." %}

[Replace with a closing paragraph.]
```

- [ ] **Step 4: Build the site**

Run:
```bash
bundle exec jekyll build
```

Expected: completes without error. If Jekyll complains that `page.slug` is undefined, fall back to `{{ page.path | split: '/' | last | replace: '.md', '' }}` in `_includes/photo.html` and rebuild.

- [ ] **Step 5: Verify the Slovenia page rendered**

Run:
```bash
test -f _site/trips/slovenia/index.html && echo "OK: page rendered" || echo "MISSING"
```

Expected: `OK: page rendered`.

- [ ] **Step 6: Verify the photo include resolved the path correctly**

Run:
```bash
grep -o 'src="/images/trips/slovenia/placeholder-1\.jpg"' _site/trips/slovenia/index.html
```

Expected: one matching line. If empty, the `page.slug` substitution failed — check `_includes/photo.html` and the slug fallback.

- [ ] **Step 7: Verify the figcaption rendered**

Run:
```bash
grep -c '<figcaption>' _site/trips/slovenia/index.html
```

Expected: `2` (one for each include in the skeleton).

- [ ] **Step 8: Commit**

```bash
git add _includes/photo.html _trips/slovenia.md
git commit -m "Add photo include and Slovenia trip skeleton"
```

---

### Task 3: Style the figure (photo + caption) in the global stylesheet

**Files:**
- Modify: `_layouts/default.html` (inside the existing `<style>` block — append before the `@media` block near line 100)

- [ ] **Step 1: Confirm the existing style block layout**

Run:
```bash
sed -n '95,110p' _layouts/default.html
```

Expected: ends with:
```
        @media (max-width: 600px) {
            body {
                padding: 20px 15px;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
```

The new rules go before the `@media` block so the media query stays last.

- [ ] **Step 2: Insert the figure CSS**

Edit `_layouts/default.html`. Add the following three rules immediately before the `@media (max-width: 600px)` block (i.e., after the `.post-meta` block):

```css
        figure.trip-photo {
            margin: 2rem 0;
        }

        figure.trip-photo img {
            width: 100%;
            height: auto;
            display: block;
            border: 1px solid #e5e5e5;
        }

        figure.trip-photo figcaption {
            margin-top: 0.5rem;
            font-style: italic;
            font-size: 0.9rem;
            color: #666;
            text-align: left;
        }
```

These match the existing palette: `#666` is already used by `.blog-date` and `.last-updated`, and `#e5e5e5` is a neutral light grey for the subtle border. Captions are left-aligned per the design decision.

- [ ] **Step 3: Build the site**

Run:
```bash
bundle exec jekyll build
```

Expected: completes without error.

- [ ] **Step 4: Verify the CSS made it into the generated page**

Run:
```bash
grep -c 'figure.trip-photo' _site/trips/slovenia/index.html
```

Expected: `3` (one for each of the three rules — Jekyll inlines the layout's `<style>` block into every page).

- [ ] **Step 5: Commit**

```bash
git add _layouts/default.html
git commit -m "Style trip-photo figures and captions"
```

---

### Task 4: Add the Trips section to the homepage

**Files:**
- Modify: `index.md` (insert a new section between the Thoughts and Interests sections)

- [ ] **Step 1: Confirm current homepage layout around the insertion point**

Run:
```bash
sed -n '22,36p' index.md
```

Expected output:
```
{% assign thoughts = site.thoughts | sort: 'date' | reverse %}
{% if thoughts.size > 0 %}
## Thoughts

I sometimes write about ideas I've explored and things I've learned; a few are listed below:

{% for post in thoughts %}
- [{{ post.title }}]({{ post.url }}) ({{ post.date | date: "%B %Y" }})
{% endfor %}
{% endif %}

## Interests

This is an incomplete list of topics I'm exploring and reading about
```

The new Trips block goes between the closing `{% endif %}` of the Thoughts section and the `## Interests` heading.

- [ ] **Step 2: Insert the Trips section**

Edit `index.md`. After the line containing `{% endif %}` (which closes the Thoughts block) and before `## Interests`, insert:

```liquid
{% assign trips = site.trips | sort: 'date' | reverse %}
{% if trips.size > 0 %}
## Trips

A few trips I've taken and written up:

{% for trip in trips %}
- [{{ trip.title }}]({{ trip.url }}) ({{ trip.date | date: "%B %Y" }})
{% endfor %}
{% endif %}

```

(Note the blank line at the end — markdown needs a blank line before the next `## Interests` heading.)

- [ ] **Step 3: Build the site**

Run:
```bash
bundle exec jekyll build
```

Expected: completes without error.

- [ ] **Step 4: Verify the Trips section rendered on the homepage**

Run:
```bash
grep -A1 'id="trips"' _site/index.html || grep -B1 -A3 '>Trips<' _site/index.html
```

Expected: shows `<h2 ...>Trips</h2>` (Jekyll/kramdown auto-generates an `id` from heading text) followed by the intro line and a `<ul>` with a link to `/trips/slovenia/`.

- [ ] **Step 5: Verify section ordering on the homepage**

Run:
```bash
grep -nE '<h2[^>]*>(Projects|Thoughts|Trips|Interests|Contact)<' _site/index.html
```

Expected: lines appear in this order — `Projects`, `Thoughts`, `Trips`, `Interests`, `Contact`. (About uses `<h2>About Me</h2>` and appears first.)

- [ ] **Step 6: Commit**

```bash
git add index.md
git commit -m "Add Trips section to homepage"
```

---

## Manual verification before pushing

After all four tasks pass:

- [ ] **Serve the site locally and click through it**

Run:
```bash
bundle exec jekyll serve
```

Open `http://localhost:4000` in a browser:
- The homepage shows a "Trips" section between "Thoughts" and "Interests" with one link: "Slovenia (June 2025)".
- Clicking the link goes to `/trips/slovenia/`.
- The Slovenia page shows the title, date, description block, the three placeholder paragraphs, and two broken-image icons (expected — no real photos yet) each with an italic left-aligned caption beneath.
- The image placeholders have a subtle 1px grey border.
- The site otherwise looks identical to before (Projects, Thoughts, About, Interests, Contact unchanged).

Stop the server with `Ctrl+C` when done.

- [ ] **Confirm Cloudflare Pages will pick this up**

`git push origin main`. Cloudflare Pages (the live host of `rahulsrivathsa.com`) is wired to this repo and rebuilds on push. Expect the new section live on `rahulsrivathsa.com` within ~60 seconds.

---

## Follow-up (not in scope of this plan)

- Rahul adds real Slovenia photos to `images/trips/slovenia/` (web-sized JPEGs, ~1600px wide max).
- Rahul replaces the placeholder paragraphs and photo `src=`/`caption=` values in `_trips/slovenia.md` with the real trip writeup.
- If photo weight in the repo becomes a problem after a few trips, revisit: external image host, Git LFS, or a build-step optimizer.
