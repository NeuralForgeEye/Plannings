Follow-up to the style-caching fix. build_excel() still takes ~106s for 24,629 rows
× 106 columns because openpyxl is slow at writing large files.

Replace the Excel writing in ExcelBuilderService (excel_builder_services.py) with
xlsxwriter:
- Create each format once with workbook.add_format() (header, odd row, even row,
  and any other styles currently used), and reuse them.
- Write whole rows with worksheet.write_row(row, 0, values, row_format) instead of
  cell by cell, where possible.
- Keep everything else identical: sheet name, column order, headers, column widths,
  frozen header if present, colors, fonts, borders, wrapping, values.
- Write to an in-memory BytesIO, so the rest of the flow (_ExcelStreamWrapper →
  StreamingResponse) stays the same.
- Keep the openpyxl version behind a feature flag so we can switch back.

Prove it:
1. Time the new build_excel() at 500, 2,000, 5,000 and 24,629 rows (same data as
   before) and show old vs new in a table.
2. Open both files and compare values and styles cell by cell; confirm they match.
3. Run test_excel_builder_services.py and confirm everything passes.
