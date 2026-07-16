# devdocs-manual-cpp

The owned C++ manual for
[devdocs.nvim](https://github.com/Derrekito/devdocs.nvim): a full
markdown docset (`index.json` + `pages-md/`) checked out as a git
submodule at `manuals/cpp`, taking precedence over any downloaded
docset.

Seeded by `:DevdocsAdopt cpp` from the [devdocs.io](https://devdocs.io)
`cpp` docset (itself built from
[cppreference.com](https://en.cppreference.com)), converted to markdown
by the plugin's structure-aware converter. From that seed onward this
tree is maintained by hand: descriptions rewritten for clarity, noise
deleted, structure changed freely. New language specs are reconciled by
adopting a fresh upstream snapshot elsewhere and diffing against this
tree — never by overwriting it.

## Layout

- `index.json` — entry name → page path map used by the plugin's
  search and `gK` lookup.
- `pages-md/**/*.md` — one markdown file per reference page.
- `STYLE.md` — the rewrite format contract; `pages-md/algorithm/sort.md`
  is the canonical rewritten page.

Rewritten pages lead with a plain-language summary and usage synopsis,
translate standardese parameters into what callers actually write,
promote gotchas out of footnotes, and preserve the full formal
declarations verbatim under a `### Reference` section. Version
applicability (C++11/14/17/20/23) is stated explicitly, and every
example on a rewritten page compiles — with printed output asserted —
via the plugin repo's `make check-examples-run`. Pages not yet
rewritten remain raw converter output; they get the same viewer
features (links, pager) either way.

## License and attribution

This work is derived from content authored by the
[cppreference.com](https://en.cppreference.com) community and is
licensed under
[CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) (see
`LICENSE`). Modifications: conversion to markdown, editorial rewrites,
restructuring, and added examples.
