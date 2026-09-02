# Agent Guide: zz-plant Profile Repo

This repository renders the GitHub profile README for the user **zz-plant** (Kanav Jain). `README.md` at the repo root is the profile; there is no application code here.

## Layout

- `README.md` — the profile itself.
- `docs/assets/` — every image the profile references, and nothing else. (It once held a `zz-plant-*` set of social/OG cards described as serving the portfolio sites; none of those sites used them — each serves its own OG image — and they were a mock project-index card, not a capture. Removed 2026-09-01.)

## Rules

1. **Assets must live in this repo.** Several of the projects the profile describes (`whether`, `blog`, `bread`, the `nextconsensus-*` repos) are private. Referencing an image from a private repo's raw URL renders a broken image for every visitor. Copy the asset into `docs/assets/` instead.

2. **Only link repos that are public.** Before adding a repo link, confirm visibility:
   ```
   gh repo list zz-plant --limit 100 --json name,isPrivate
   ```
   Private work belongs in the profile as a described project with a link to its live site, not as a repo link that 404s.

3. **Numbers in the profile must be re-checked, not carried forward.** Preset counts, essay counts, and tool counts drift as the projects ship. Verify against the live surface or the source repo before editing a line that contains one.

4. **Captures show real software.** Screenshots and clips are recorded from the running app or the live site. Do not substitute abstract title-card animations — they demonstrate nothing.

5. **NextConsensus copy is governed elsewhere.** Load the `nextconsensus-public-surface-audit` skill before changing the NextConsensus section. "Forecast" is the canonical, authorized verb; "predict", "early warning", and capability-tier words like "calibrated" or "proven" are banned on public surfaces.

6. **No application source code.** This stays a profile and asset repo.

## Regenerating captures

Clips are recorded with Playwright against the live sites, then encoded with a two-pass ffmpeg palette:

```
ffmpeg -framerate 9 -i f%03d.png -vf "scale=720:-2:flags=lanczos,palettegen=max_colors=128:stats_mode=diff" pal.png
ffmpeg -framerate 9 -i f%03d.png -i pal.png -lavfi "scale=720:-2:flags=lanczos [x]; [x][1:v] paletteuse=dither=bayer:bayer_scale=4:diff_mode=rectangle" -loop 0 out.gif
```

Keep each asset under ~3.5MB; GitHub proxies README images and large GIFs stall the page.
