# vcs-robotics.com

The website for VCS Robotics at Valley Christian Schools (Chandler, AZ): FTC 2844 Valley X
(varsity), FTC 12841 +3 Robotics (JV), and FRC 8848 BLU CRU.

It's a [Jekyll](https://jekyllrb.com) site that GitHub Pages builds automatically. There's no build
step to run and nothing to deploy by hand: push to the publishing branch and the site updates.

## Editing content (no HTML needed)

Most updates happen in the YAML files in `_data/`:

| To change... | Edit |
| --- | --- |
| Awards (new season, new award) | `_data/awards.yml` |
| Home page "Latest from the field" posts | `_data/updates.yml` (newest first; home shows 3) |
| Sponsors, current and past | `_data/sponsors.yml` |
| Team bios, facts, highlights, robots, photos | `_data/teams.yml` |
| Mentors | `_data/mentors.yml` |
| Student rosters (shown on team pages) | `_data/roster.yml` |
| Mark a team inactive | `status: hiatus` (plus `status_note`) on that team in `_data/teams.yml` |
| Links to ValleyLib, valleyVis, Apex Scout | `valleylib_url`, `valleyvis_url`, `apex_scout_url` in `_config.yml` |
| Season name in the header ribbon | `current_season` in `_config.yml` |
| Contact email | `contact_email` in `_config.yml` |

Award counts on the home, team, and awards pages are calculated from `awards.yml`, so they update
on their own.

**Adding a sponsor logo:** put the image in `assets/img/sponsors/`, then add
`logo: /assets/img/sponsors/filename.png` to that sponsor's entry. Sponsors without a logo show their
name instead.

**Adding a photo:** save it twice in `assets/img/photos/`, as `name.webp` and `name.jpg` (about 1600px
wide max), then use it in a page with:

```liquid
{% include photo.html name="name" alt="Describe what's in the photo" w=1600 h=1067 caption="Optional caption" %}
```

`w` and `h` are the image's real pixel size; they stop the page from jumping while it loads. Don't
publish anything with a phone number, address, or other personal details visible in it.

**Brand assets** live in `assets/img/brand/`: white versions of the program and Valley X logos (made from
the blue originals) and the X mark used in the header and favicon. The brand blue is `#0058B2`.

**New season checklist (each September):**
1. Update `current_season` in `_config.yml`.
2. Add a new season block at the top of each FTC team in `_data/awards.yml`.
3. Move last season's sponsors to `past` in `_data/sponsors.yml` if they aren't returning.
4. Update the robot section on `engineering.html`.

## Pages

| URL | File |
| --- | --- |
| `/` | `index.html` |
| `/teams/`, `/teams/2844/`, `/teams/12841/`, `/teams/8848/` | `teams/` |
| `/awards/` | `awards.html` |
| `/engineering/` | `engineering.html` |
| `/outreach/` | `outreach.html` |
| `/sponsors/` | `sponsors.html` |
| `/join/` | `join.html` |

Layouts live in `_layouts/`, shared pieces (header, footer, cards) live in `_includes/`, and the stylesheet
is `assets/css/main.css`. Brand colors are CSS variables at the top of that file.

## Preview locally

Requires Ruby 3.x.

```bash
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000. The `Gemfile` pins the `github-pages` gem, so local builds match what
GitHub runs.

## Hosting setup (one time)

1. **Repo visibility.** GitHub Pages from a private repo depends on the organization's plan. If the
   ValleyX org is on GitHub Free, make this repo public (it contains no secrets).
2. **Turn on Pages.** Go to Settings → Pages → Build and deployment → Source: *Deploy from a branch*,
   then pick `main` and `/ (root)`.
3. **Custom domain.** In the same screen, enter `vcs-robotics.com` and save. The `CNAME` file in this repo
   already matches. Then verify the domain under the org's Settings → Pages so no one else can claim it.
4. **DNS** at the domain registrar:

   | Type | Name | Value |
   | --- | --- | --- |
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | AAAA | @ | 2606:50c0:8000::153 |
   | AAAA | @ | 2606:50c0:8001::153 |
   | AAAA | @ | 2606:50c0:8002::153 |
   | AAAA | @ | 2606:50c0:8003::153 |
   | CNAME | www | valleyx.github.io |

5. Once the certificate is issued (can take up to an hour), check **Enforce HTTPS**.
6. Update the website field on the FTC 2844 and 12841 FIRST team profiles. They still list the old
   valleyx2844.com, which no longer resolves.
