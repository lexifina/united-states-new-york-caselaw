# US case law: The New York Court of Appeals

Part of [lexifina/global-law](https://github.com/lexifina/global-law), one git history for every country's law: this is the case law of the New York Court of Appeals, United States, which global-law reads at `united-states/new-york/caselaw/`.

Every published opinion of the New York Court of Appeals in the Caselaw Access Project, as a git history: one file per decision, one commit
per decision, dated the day it was decided. 90,472 decisions from 1800-12-02 to the end of the source (New York Reports (N.Y., N.Y.2d; published under contract) and, after N.Y.2d, the North Eastern Reporter (West)).

## Questions as git commands

| Question | Command |
|---|---|
| A decision by its citation | `grep -P '^<citation>\t' decision-index.tsv` then `cat <path>` |
| Decisions given in a window | `git log --format='%(trailers:key=In-force,valueonly,separator=) %an' \| awk '$1 >= "1973-01-01" && $1 <= "1973-01-31"'`: read the `In-force:` trailer, not the commit date (see Corrections), or use the global-law tools |
| Decisions citing 11 U.S.C. § 1182 | `cat judgments/united-states/federal/usc/title-11/1182.jsonl` |
| The section as the court applied it | each overlay row's `law_commit` is the section's own version in the US Code record on the day decided (or `law_record_from` when the decision is older than the record) |
| Who wrote a line? | `git blame decisions/...`: each line is credited to the decision (court and citation) |

## Dates and authors
The commit date is the day the decision was given, at noon US Eastern (`In-force:` and `Decided:`). Git cannot hold dates
before 1970, so earlier decisions are committed on 1970-01-01, a second apart, and `In-force:` holds the real day. The
author is the court and the official citation, at an address that cannot exist. These are court rulings, not laws: every
commit is `Kind: court`.

## Source
The Caselaw Access Project's bulk data (Harvard Law School Library), dedicated to the public domain under CC0; court
opinions are not copyrightable (17 U.S.C. § 105; Georgia v. Public.Resource.Org, 2020). Each file keeps its source URL.
Text is CAP's OCR, unchanged. The head matter CAP holds (syllabus, headnotes, counsel lists, the
reporter's summary) is left out: in these reporters it is written by the publisher, not the court. Each file's front matter
gives the reporter and `precedential_status: published` (every decision here was printed in a reporter; CAP records no
other status).

## What it does not show
Decisions after the end of CAP's coverage (its last volumes are of 2018-2020): CAP is a closed archive, so no nightly
runner appends here. Unpublished decisions not in the reporters, the head matter, and statute citations to US Code titles
the record does not hold.

## Added from the CAP bulk data (2026-11-08)
18,691 decisions this repo did not hold, from the Caselaw Access Project's bulk files (N.Y., N.Y. 2d, N.Y.3d, N.Y. St. Rep., N.E.3d, N.Y. Crim., How. Pr., Silv. Ct. App., Trans. App., Abb. Ct. App., Keyes, Lock. Rev. Cas. and 19 more): every decision of
its courts in CAP that was not here by its CAP id, nor as the same case (day and name) read from another reporter. Each
was appended as its own commit, dated the day decided, after the decisions already here (the history is append-only, so
they are not in date order; the tools read each decision's own date). Their head matter, the publisher's text, is left
out. Now 109,163 decisions.

## Corrections (2026-11-09)
An independent check of the CAP case-law repos found data defects; they are fixed here by new commits on top of the
history (nothing is rewritten). Now 111,440 decisions.

- 2,277 decisions added, each as its own commit dated the day decided, in date order: decisions with text the first build missed (volumes whose extract was lost, decisions merged with a different decision of the same day, docket and name, decisions CAP files under no court name or under another place's jurisdiction).
- 1,034 files had no text: CAP holds no opinion text for them, only head matter. Each now gives the head matter under `## CAP head matter` (`text_source` in the front matter), in a `Kind: correction` commit on the same file (`Corrected:`, `Corrects:` the commit that added it).
- decision-index.tsv: 14,151 rows added (the citations and CAP ids of decisions held here under another reporter, and of the new decisions), 52 rows that gave a citation to a different decision removed.
- `decision-relations.tsv` lists 59 files as `same-decision-as` another file here: decisions the CAP append added although the same decision (same day, docket or name, and text) was already here. Both files are kept; the tools read the relation.
- `decision-dates.tsv` lists the 5,924 decisions whose CAP date gives only the month or the year (precision `month` or `year`); they are committed on the first day of it, and In-force says that day.

Dates in the commit history: git cannot hold dates before 1970, so a decision before 1970 is committed from 1970-01-01 noon UTC-5, a second apart.
Here 72,260 decisions are before 1970, so 29,061 of their commits are dated 1970-01-02 or later, 5,712 of them in or after April 1970 (an append continued an earlier count). The commit date is not the day decided: `git log --since/--until` over 1970 mixes them with the decisions of 1970. Read the `In-force:` trailer (as the window recipe above now does) or use the global-law tools.
