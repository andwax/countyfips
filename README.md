# countyfips
`countyfips` is a simple Stata crosswalk program for adding various county-level identifiers (name, FIPS, Census codes) that may be missing from your dataset. The purpose of this program is to facilitate merging datasets that do not share a common identifier.

## Prerequisites

**Stata:** `countyfips` is compatible with Stata version 12.1+. It may be compatible with previous versions, but it has not been tested in those environments.

## Installation

**Github (for Stata v13.1+):** Run the following via the Stata command line.
```Stata
net install countyfips, from(https://raw.github.com/cbgoodman/countyfips/master/) replace
```

**Github (for Stata v12.1+):** Download the [zipped file](https://github.com/cbgoodman/countyfips/archive/master.zip) of this repository. Unzip on your local computer. Run the following code via the Stata command line replacing <local source> with the location of the unzipped repository on your computer.
```Stata
net install countyfips, from(<local source>) replace
```

## Using countyfips

`countyfips` offers four methods of merging using either county names, five-digit county FIPS codes, the combination of two-digit state FIPS and three-digit county FIPS codes, or the combination of two-digit state Census codes and three-digit county Census codes.

Merging with `name` where *county* is the name of county requires either `statefips` or `statecode` to be specified
```Stata
. countyfips, name(county) statefips(stfips)
```
or
```Stata
. countyfips, name(county) statecode(stcode)
```

Merging with `fips`
```Stata
. countyfips, fips(county)
```

Merging with `statefips` and `countyfips`
```Stata
. countyfips, statefips(stfips) countyfips(cofips)
```

Merging with `statecode` and `countycode`
```Stata
. countyfips, statecode(stcode) countycode(cocode)
```

By default, `countyfips` will generate a new variable, `_merge`, to indicate the merged results.  If you do not want to create this variable, specify `nogenerate`.
This will keep matched observations and unmatched master observations.
```Stata
. countyfips, fips(county) nogenerate
```

## Limitations
* Using `countyfips` with the `name` option requires specific formatting of the `name` variable. County names must *not* include "county" after the name. County names beginning with "Saint" such as Saint Louis must be abbreviated to "St."
* FIPS codes are available for U.S counties, county equivalents, and territories. Census codes are only available for counties or county equivalents (such as independent cities).
* FIPS codes for Virginia independent cities as well as merged county and independent cities (as is the practice for some areas in BEA data) are included. Using `countyfips` with the `name` option will require extremely particular formatting. Additionally, only the `statefips` option will work when merging with `name`.
* `countyfips` uses the more recent FIPS code for Miami-Dade county (12086) rather than the FIPS code for Dade county (12025). The Census state (10) and county (13) codes remain unchanged.

## Bugs
Please report any bugs [here](https://github.com/cbgoodman/countyfips/issues).
