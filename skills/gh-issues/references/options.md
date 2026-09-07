# Invocation options

The existing /gh-issues argument vocabulary is preserved. Select only the user-
requested mode; these flags do not bypass task authority or host policy.

| Option           | Default / meaning                                                        |
| ---------------- | ------------------------------------------------------------------------ |
| owner/repo       | optional; resolve current remote when absent, never guess                |
| --label          | optional issue label                                                     |
| --limit          | 10 issues per requested poll                                             |
| --milestone      | optional title, resolve to API milestone number                          |
| --assignee       | optional, @me resolves authenticated user                                |
| --state          | open; supports open/closed/all                                           |
| --fork           | optional push repository; source repository owns issues/target PR        |
| --dry-run        | false; fetch/display only, no workers or state mutations                 |
| --yes            | false; skip selection confirmation for the authorized issue set          |
| --reviews-only   | false; inspect/address specified review scope instead of issues          |
| --watch          | false; recurring polling only when expressly requested                   |
| --interval       | 5 minutes when watch is authorized                                       |
| --cron           | false; scheduler-invoked mode, not scheduler creation or extra authority |
| --model          | unspecified; retain default unless user supplies exact model             |
| --notify-channel | optional explicit final-summary destination; no extra status posts       |

Carry SOURCE_REPO, PUSH_REPO, FORK_MODE, selected issue IDs and verified base/head.
Do not expose credentialed remote values in this state. --fork does not create a
fork or authorize push. An unspecified issue list or ambiguous publication scope
must be resolved before the affected operation, without repeating prior consent.
