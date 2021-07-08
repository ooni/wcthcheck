# OONI Web Connectivity Test Helper Check

This repository contains code to compare the old and the new versions
of OONI's Web Connectivity test helper (wcth). We want to understand whether
we can safely replace the old wcth with the new wcth.

## Existing test helpers

The old test helper implementation is
[ooni/backend](https://github.com/ooni/backend/tree/1164a70/oonib), while
the new implementation is instead at
[ooni/probe-cli](https://github.com/ooni/probe-cli/tree/3747598b/internal/cmd/oohelper).

At the moment of writing this README file, the old test helper is
deployed at `https://wcth.ooni.io/`, and the new test helper is instead
available at `https://1.th.ooni.org/`.

## Test lists

The `alltestlists.txt` file contains the URL present in all the test lists
at [citizenlab/test-lists](https://github.com/citizenlab/test-lists) as
of June 6th, 2021. We removed redundant entries.

## Code to query the test helpers

The [ooni/probe-cli](https://github.com/ooni/probe-cli) repository contains
code to query a wcth, called `oohelper`. We build this code with the
`buildoohelper` script.

The `run` script takes care of calling `buildoohelper` and then uses the
`oohelper` binary to query both test helpers for every entry in the
`alltestlists.txt` file.

## Database containing results

The `run` script stores results into a `database.sqlite3` file. This
script assumes Python >= 3.8.

The code assumes that this file already exists. If it does not exist, then
you can create one using `sqlite3`. This is the database schema:

```SQL
CREATE TABLE results(date text, backend text, target text, json text);
```

The `date` field contains the date when we queries a test helper. The
`backend` field is the URL of the test helper. The `target` field is
a URL from `alltestlists.txt`. The `json` field is the JSON result returned
by a test helper for a given input URL.

The objective of this analysis is to compare the JSON returned by each
test helper and identify which are the differences and why.

## Releasing a database

Because it takes quite a long time to build a database and because the
database itself is quite large, we publish a snapshot of the database
with over 80\% of the URLs measured as a release of this repo. Then you
can download the database and just perform the results analysis.

## Results analysis

To this end, we use the `compare` script. This script requires Python >= 3.8.


