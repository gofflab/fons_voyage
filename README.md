# Fons Voyage

> Dispatches from the neurogenic niche. Cells, sequences, and cephalopods.

The lab blog of the [Goff Lab](https://igm.jhmi.edu/) at the Johns Hopkins
School of Medicine. Built with [Nikola](https://getnikola.com/) and deployed
to GitHub Pages.

**Live site:** https://gofflab.github.io/fons_voyage/

---

## Local development

The environment is defined in `environment.yml` and managed with
[conda](https://docs.conda.io/) / [mamba](https://mamba.readthedocs.io/).
[Miniforge](https://github.com/conda-forge/miniforge) (which bundles
`mamba` + conda-forge defaults) is the recommended installer.

```bash
# create the env
mamba env create -f environment.yml      # or: conda env create -f environment.yml

# activate it (run this each new shell)
mamba activate fons_voyage

# later, after pulling changes to environment.yml
mamba env update -f environment.yml --prune
```

### Common commands

```bash
nikola new_post -t "My title"          # new Markdown post under posts/
nikola new_post -f markdown -t "Title" # explicit format
nikola new_page -t "About"             # new standalone page
nikola build                           # build site into output/
nikola serve -b                        # build + serve locally on :8000
nikola check -l                        # check for broken links
nikola auto                            # build on file change + livereload
```

The built site lives in `output/` (gitignored).

## Writing posts

- **Markdown** posts go in `posts/*.md`.
- **reStructuredText** in `posts/*.rst`.
- **Jupyter notebooks** in `posts/*.ipynb` (rendered directly).
- Front-matter (Markdown) lives in a top-of-file HTML comment — see
  `posts/welcome-aboard.md` for the canonical example.

Equations use **KaTeX** (`USE_KATEX = True`). Wrap inline math in `$...$`
and display math in `$$...$$` after enabling `KATEX_AUTO_RENDER` (see
`conf.py` for the snippet).

## Theme

A child theme `fonsvoyage` lives in `themes/fonsvoyage/`, inheriting from
the bundled `bootblog4`. The only template override is `base.tmpl` (to
inject the tagline under the title). All visual styling is in
`files/assets/css/custom.css` (the "Deep Ocean" palette).

To tweak colors, edit the CSS variables at the top of that file:

```css
:root {
    --fv-deep: #0d4d5e;   /* primary teal */
    --fv-coral: #e07a5f;  /* link accent */
    /* ... */
}
```

## Comments (giscus)

Comments are powered by [giscus](https://giscus.app/), which stores each
comment thread as a GitHub Discussion in this repo. To activate (one-time):

1. **Settings → General → Features → Discussions** (enable).
2. Install the [giscus GitHub App](https://github.com/apps/giscus) on
   `gofflab/fons_voyage`.
3. The Discussion category, repo IDs, and mapping mode are already set
   in `conf.py` under `GLOBAL_CONTEXT["giscus_config"]`. To change them
   later (e.g. switch category), regenerate the config at
   [giscus.app](https://giscus.app/) and paste the new `data-*` values
   into that dict.

Comments inherit the visitor's light/dark preference automatically
(`data-theme: preferred_color_scheme`).

## Deployment

Pushes to `main` trigger `.github/workflows/deploy.yml`:

1. Provision a micromamba env from `environment.yml` (cached between runs).
2. Run `nikola build`.
3. Upload `output/` as a Pages artifact.
4. Deploy to GitHub Pages.

### One-time GitHub setup

In the repo on github.com:

1. **Settings → Pages → Source:** select **GitHub Actions**.
2. **Settings → Actions → General → Workflow permissions:** ensure
   "Read and write permissions" is enabled (or rely on the per-job
   permissions in the workflow).
3. Push to `main` — the first deployment will provision the site at
   <https://gofflab.github.io/fons_voyage/>.

### Custom domain (later)

To use a custom domain (e.g. `fonsvoyage.org`):

1. Add a `CNAME` file in `files/` containing just the domain.
2. Update `SITE_URL` in `conf.py`.
3. Configure DNS (ALIAS / CNAME → `gofflab.github.io`).
4. Enable HTTPS in Settings → Pages.

## Repo layout

```
.
├── conf.py                 # Nikola site config
├── posts/                  # blog posts (.md / .rst / .ipynb)
├── pages/                  # standalone pages (.md / .rst / .ipynb)
├── images/                 # source images (resized at build time)
├── files/                  # raw files copied verbatim to output/
│   └── assets/css/custom.css   # Deep Ocean palette
├── themes/fonsvoyage/      # custom child theme
├── output/                 # build output (gitignored)
└── .github/workflows/      # GitHub Actions
```
