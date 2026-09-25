The project 94 Excel export (≈2,000 rows × ~400 columns) takes ~13 seconds end to end.
Find out exactly where those 13 seconds go. Don't change any project code; measure only.
Use temp scripts, delete them afterward, and confirm git status is clean.

Measure each phase separately (3 runs each, report the average):
1. DB query + all data loading (main query, HITL/feedback details, label/field/
   source/origin maps, formatter config)
2. build_export_schema / JSON flattening into rows
3. build_excel(). Also tell me which library it uses right now (openpyxl or
   xlsxwriter) and whether it still writes/styles empty cells
4. Total server time: call the real local API with curl and record time to
   first byte and total time
5. Browser time: in DevTools → Network → the export request → Timing, record
   "Waiting (TTFB)" and "Content Download". If you can't do this step yourself,
   tell me exactly what to click and I'll send you the numbers.
Also report: exact row count, column count, filled cells vs total cells, file size in MB.

Output:
- One table: phase | seconds | % of total
- The top 2 slowest phases, with the specific function/lines responsible and why
- For each of those 2, the fix you'd recommend and its expected time saving
