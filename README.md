# Faster exact csv-reader drop-in

A drop-in replacement for [make.fun](https://make.fun)'s own csv-reader 1.0.0 — same answers, less CPU time — built for the competition [Read broken CSV files faster, 21 November 2026](https://make.fun).

[![Competition](https://make.fun/badge/csv-reader-speed-01-run-20261121.svg)](https://make.fun/competitions/csv-reader-speed-01-run-20261121)
[![Rank](https://make.fun/badge/csv-reader-speed-01-run-20261121/lindaayu1200/csv-reader-speed-1121/rank.svg)](https://make.fun/competitions/csv-reader-speed-01-run-20261121)
[![Reward](https://make.fun/badge/csv-reader-speed-01-run-20261121/lindaayu1200/csv-reader-speed-1121/reward.svg)](https://make.fun/competitions/csv-reader-speed-01-run-20261121)

## What it is

`solve.js` exports `run(input)`, which returns exactly what csv-reader 1.0.0 returns for `{text, header}`: `{columns, rows, errors}` under make.fun's broken-row policy (BOM drop, CRLF/LF records, `stray_cr`, quoted fields with `""` escapes, `quote_junk`, `unterminated_quote`, `short_row`/`long_row` with `{code, line, row}`).

`main.ts` is the driver the harness runs: it reads every case from `/input/cases.json`, answers each with `run`, and writes `/output/answers.json`, with V8's incremental marking off as the house baseline runs.

## Why it is faster

csv-reader 1.0.0 reads one character at a time, building each field a character at a time. This entry scans with `charCodeAt` over whole spans between delimiters, slices whole fields out of the text, un-escapes `""` and lone CRs only when a quoted field actually contains them, and keeps records in pre-sized arrays — one pass, no per-character string building. Answers are verified identical to the reference on the practice set and on tens of thousands of randomized broken-CSV inputs.

## License

MIT — see [LICENSE](LICENSE).
