# Page style — how this manual diverges from cppreference

cppreference is written for language lawyers and implementers; this
manual is written for someone with code open in the next split. Pages
are rewritten to be understood on first read, without losing the facts
that matter (versions, complexity, UB). `pages-md/algorithm/sort.md`
is the canonical example of the format.

## Page shape

1. `# std::name` — title unchanged (search and gK key off entry names).
2. **Plain-language summary** — 1–3 sentences: what it does, when you
   would reach for it, and the headline caveat. No template syntax
   here.
3. **Usage synopsis** — a short ` ```cpp skip ` fence showing how calls
   actually look (`std::sort(v.begin(), v.end(), comp);`), one line per
   meaningfully-different form, each annotated with `// (since C++NN)`
   where gated. This replaces the wall of template declarations as the
   first thing the reader sees. Tag it `skip`: it is a synopsis, not a
   compilable example.
4. `### What you provide` — parameters in human terms. Translate
   standardese: "Compare must meet the requirements of Compare"
   becomes what the reader must actually write ("a function taking two
   elements, returning true if the first belongs before the second;
   use `<`, never `<=`").
5. `### Guarantees and costs` — complexity, iterator/reference
   invalidation, stability, exception behavior. Keep every hard fact;
   state each in one plain sentence.
6. `### Gotchas` — the ways real code goes wrong (UB from a bad
   comparator, dangling references, moved-from access, …). Promote
   these out of footnotes; this section earns the rewrite.
7. `### Example` — one compact, compilable program with a ` ```text `
   output fence. Task recipes live in the plugin's `notes/` overlay
   (appended at view time) — don't duplicate them here; keep the
   page's example minimal and orienting.
8. `### Reference` — the full formal declarations, preserved verbatim
   from upstream in a ` ```cpp skip ` fence, followed by any
   remaining precise material worth keeping (type requirements,
   defect-report notes) in condensed form. Precision is kept, but it
   lives at the bottom, not the top.
9. `### See also` — keep, prune to entries a reader plausibly wants
   next, one line each with the version tag if gated. Write the bare
   entry name in **bold** or `backticks`: any such span that resolves
   in the index becomes a followable link in the viewer (Tab hops,
   CR/K follows), so inventory lists and See-also sections are
   navigation, not decoration. Keep version tags outside the span
   (`**sort**` `(C++20)` — not `**sort (C++20)**`).
10. Final `*Source: <cppreference URL>*` line — never remove
    (CC-BY-SA attribution).

## Rules

- Never invent facts. Every claim about complexity, versions,
  invalidation, or UB must come from the upstream text being
  rewritten. When upstream is ambiguous, keep its wording.
- Version tags (`(since C++17)`, `(until C++20)`) survive every
  rewrite. State the standard in prose when it drives a decision
  ("parallel overloads need C++17").
- Delete what serves no reader: "Possible implementation" pointers,
  defect-report tables (fold anything behavior-relevant into a
  sentence), boilerplate exception paragraphs repeated on every
  algorithm page (state the terminate-on-throw rule once, plainly).
- Prose hard-wrapped at 80 columns; headings are `###` (h2 reserved,
  `#` is the title).
- Example fences must compile: run
  `python scripts/check_examples.py <page files>` from the plugin
  root (the checker takes explicit paths). Pin standards with
  ` ```cpp c++20 `; `skip` is only for synopsis/declaration fences.
