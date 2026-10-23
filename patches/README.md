# Local patches to SQLite

`c/sqlite3.c` is the official amalgamation with these patches applied. Each one is a diff against the SQLite source tree of the release in `sqlite3.h`. Drop a patch once a SQLite release has an equivalent fix, and remove the patch from `c/sqlite3.c` by copying the pristine amalgamation back in.

| Patch | Why | Reported |
| --- | --- | --- |
| `fts5-linear-parse.patch` | FTS5 built an `AND`/`OR` chain of N terms in O(N²) time, copying every earlier child into a new node for each term, without checking the interrupt flag. 100,000 terms took 3.2 s, 400,000 took 58 s (Apple M4 Max, SQLite 3.54.1). With the patch, 51 ms and 215 ms. Checks: the 148 FTS5 test files of the SQLite source tree (0 errors), and 30,000 random queries that give the same results as the unpatched build, under AddressSanitizer and UndefinedBehaviorSanitizer. | [SQLite forum, 2026-10-23](https://sqlite.org/forum/forumpost/14481bd32ddc58a9da6c335940934a9489be3a7d1506cb1ee3461e2ee6599499) |
