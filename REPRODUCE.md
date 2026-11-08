# Reproducing this repository

```
python tools/cap_bulk_fetch.py <cap-bulk>                  # every volume zip of static.case.law, verified
python tools/cap_bulk_extract.py <cap-bulk> <parsed> --reporters <reporters> --courts '<court regex>' [--after <day>]
python tools/build_state.py new-york <parsed>/<reporter>,... <empty folder> --today 2026-11-08
```

Input: the Caselaw Access Project's bulk files on static.case.law (CC0): VolumesMetadata.json and the volume zips it lists,
fetched 2026-11-08 at 16 to 32 parallel streams (about 12 MB/s), backing off on any error. static.case.law's robots.txt
disallows all paths (`User-agent: *` / `Disallow: /`); its documentation offers these files for bulk download, and they
were fetched as such, with no listing page crawled. Gives 90,472 decision commits and one documentation commit.
