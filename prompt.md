I need you to investigate a slow Excel export in our O2A codebase and give me a complete picture with real measurements. Use Agent mode: read the code, run things, and report facts with file paths and line numbers. Don't guess — if you can't find or measure something, say so and why.

## The problem
After a QA use case's agents run, each record's question, answer, and reasoning is saved and shown on the Feedback and Reports pages. Product Owners pick a date range on the Reports page (7d, 30d, all, or custom) and download an Excel file. With thousands of records, the download takes about a minute. I want to fix this while keeping the same flow (UI → API → DB → Excel) and without storing data anywhere except the database.

## What I need from you
1. **Current flow:** trace the download end to end, from the button click through the API, the DB query, Excel generation, and the response back to the browser. Include which Excel library is used, whether the file is built all at once or streamed, whether data is loaded fully into memory, and any N+1 queries, row limits, timeouts, or proxy/gateway settings on this path.
2. **Data shape:** the source table(s), which columns go into the Excel file (list them and give the total count), the DB engine, and the existing indexes.
3. **Real numbers:** connect to the dev/UAT database and measure the record counts per QA use case (top 20); the largest use case's counts for 7d, 30d, and all time; average records per day; the average and max length of question, answer, and reasoning; the estimated data size; and whether the export query uses an index (EXPLAIN).
4. **Timing:** run the existing export for the largest use case at 7d, 30d, and all time, and time each step: DB query, processing, Excel build, total. Also record the file size and peak memory. Put the results in a table.
5. **Diagnosis:** the top causes of the slowness, based on the evidence, ranked by how much time each one costs.

## Rules
- Don't modify project code. Any test scripts go in a temp folder and get deleted afterward; confirm git status is clean at the end.
- Database: read-only (SELECT/EXPLAIN only), dev/UAT only, lightweight queries. Tell me which environment you used. If the query touches Teradata, skip the heavy all-time runs.
- Keep the report concise: headings per section, tables for numbers, short code excerpts only where they prove a point.
