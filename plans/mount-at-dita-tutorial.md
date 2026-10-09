# Serve the tutorial site at /dita-tutorial

The site moves from `dogsbay.ai/dogsbay-xml-dita-tutorial-docs/` to `dogsbay.ai/dita-tutorial/`, and the repository's default branch becomes `main`. The repository keeps its name.

## Why

The mount path is the URL a learner meets at the end of a video, and some of them will type it. `dogsbay-xml-dita-tutorial-docs` is 30 characters that say "dogsbay" twice, carry both "xml" and "dita", and end in `-docs` — which tells a reader nothing, because the site is the docs.

The repository name is a different audience. It pairs with its content repository `dogsbay-xml-dita-tutorial` and follows the `<project>-docs` pattern of `dogsbay-xml-docs`, so the four sort together in a GitHub listing. Nothing requires the two to agree: the Workers rule is that the repo name, the worker name and `wrangler.jsonc`'s `name` match each other, and the mount path comes from `site.url`.

Now is the moment to decide it. Nothing is deployed, and the path appeared in exactly two committed places outside this repository.

## No collision

`/dita-tutorial` was free: no `wrangler.jsonc` in the workspace routed it, neither the apex site nor the blog had content there, and both `/dita-tutorial` and `/dita-tutorial/` answered 404. The apex `dogsbay.ai/*` route does not block it — Cloudflare prefers the more specific route, which is how `/blog` already works.

## What changed

`site.url` in `dogsbay.config.yml` is the single source: `dogsbay site build` regenerates the routes, the internal links, the Pagefind output path and the mount step from it.

Two generated files are preserved across builds rather than rewritten, so both were edited by hand, and `dogsbay site build` warns about the first: `astro/package.json`'s build script (the Pagefind `--output-path`) and `astro/wrangler.jsonc`'s two route patterns.

The README's Deploying section now documents the Workers Build trigger, which it had never mentioned, and states that the mount path and the name differ deliberately — so that the next person to notice does not "fix" it by editing `astro/`.

Outside the repository, two links: `README.md` in `dogsbay-xml-dita-tutorial`, and `content/getting-started/learn-dita.md` in `dogsbay-xml-docs` (whose `astro/` is regenerated with it).

## Stale output removed

Four `astro/public/part-*/llms.txt` files were orphans. The generator no longer writes them: their headings name a nav structure that no longer exists ("Part 2: Maps and publishing", where `nav.yml` now has "Core: Maps and reuse"). They would have shipped the old path in absolute URLs. Nothing links to them.

## Verified

| Claim | How |
|---|---|
| `/dita-tutorial` collides with nothing | no route in any workspace `wrangler.jsonc`; no content in the apex site or blog; both URL forms 404 live |
| the production build works | `npm run build` in `astro/`: 44 HTML files, Pagefind indexed 43 pages into `dist/dita-tutorial/pagefind` |
| the mount is right | everything under `dist/dita-tutorial/`, `_headers` lifted to `dist/` root, no `dist/dogsbay-xml-dita-tutorial-docs/` |
| pages and search resolve | served `dist/` as the assets root: `/dita-tutorial/`, a stage page, `/dita-tutorial/pagefind/pagefind.js` and `/_headers` all 200 |
| no old path survives as a URL | every remaining mention in `dist/` is the GitHub repo URL (43) or a sibling directory in a verification code block (10) |
| the site audit is clean | `dogsbay site check`: 19 rules, no issues |

## Not in this change

- Creating the GitHub repository, adding the remote, and connecting Workers Builds — outside this machine.
- The generator defect below.

## Found, not fixed: the root llms.txt

`astro/public/llms.txt` should index the whole site. It contains only the "Optional modules" section, and is headed `DogsBay DITA tutorial — Optional modules`.

A section whose pages all sit in one directory gets its `llms.txt` there (`part-1-topics/`, `practice/`, `reference/`). "Optional modules" draws its pages from five directories, so it has no single home and lands at the root, overwriting the site index. This is a `dogsbay-ssg` defect, it predates this change, and it affects `dogsbay-xml-docs` too wherever a section spans directories.
