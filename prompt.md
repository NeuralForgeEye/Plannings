In excel_builder_services.py (ExcelBuilderService, around lines 1504–1515), the
_fill, _border, _font and _align helpers create brand-new openpyxl style objects
for every cell. That's ~10 million objects for our largest export and the reason
build_excel() takes ~13.5 ms per row.

Fix: create each format once and reuse it for every cell. Styles that are always
the same (header, alternating row fills, border, alignment) become constants.
Styles that depend on data or config (colors etc.) go through a cache
(e.g. functools.lru_cache), so any new combination is created once and reused
automatically — this must work for every use case, including future ones,
with no per-use-case changes.

Rules:
- The Excel output must look exactly the same: same colors, fonts, borders,
  wrapping, column widths, values.
- Only change the styling code. Don't touch the query, data loading, or API.

Prove it:
1. Time build_excel() before and after on the same data you measured before
   (project 5 / ingestion 23: 500 rows, 2,000 rows, and the full 24,629 rows).
2. Compare the old and new files cell by cell (values and styles) and confirm
   they're identical.
3. Show me the before/after table and the diff of the changes.
