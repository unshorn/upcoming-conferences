The purpose of this project is to build a static web site with information about upcoming conference deadlines in the field of software engineering, and related fields.

# The Conference Database


The website is based in a simple database represented as a JSON file. The website will be static, hosted on GitHub, updates to this database will be made via commits to this file.



## Initial Database

The initial input is a list of conferences provided here.

### Software Engineering

ICSE, FSE, ASE, ICSME, SANER, MSR, APSEC

### Programming Languages

PLDI, POPL, OOPSLA, ECOOP, ICFP

### Security

USENIX Security , CCS, ESORICS, TrustCom, S&P, ASIACCS, Euro S&P

## Format

The database consists of a JSON file `conferences.json` that has the following structure: 

- the file contains a list of JSON objects
- each conference is represented by a JSON object with the following fields:
   - name: the full name of the conference, such as "International Conference on Software Engineering"
   - acronym: the acronym , such as "ICSE"
   - url - the web page if there is one for the entire series
   - see-also: a list of related URLs
   - core-rank: an object containing the latest core rank, the ranking system used (like ICORE2026, see https://portal.core.edu.au/conf-ranks/1209/), and a URL to the CORE listing
   - a field of research, initially one of: "software engineering", "security" and "programming languages"
   - the sponsoring information, such as "IEEE" or "ACM"

   
      
# The Deadline Database

From the conference database, search the web to create a database of upcoming deadlines. Store the information in a database. This database consists of a JSON file `deadlines.json` that has the following structure: 

- the file contains a list of JSON objects
- each conference  deadline is represented by a JSON object with the following fields:
   - name: the full name of the conference for the respective year, such as "48th International Conference on Software Engineering"
   - acronym: the conference acronym, such as "ICSE"
   - acronymx: the conference acronym including the year, such as "ICSE'26"
   - url: the web page of the conference
   - track: the name of the track, such as "research", "industry", "NIER"
   - if there are several submission rounds, treat them as tracks, such as "research (submission round 1)"
   - submission-deadline: the submission deadline
   - estimated: boolean, `true` if the submission deadline date was inferred or estimated rather than taken from an official source; omit or set `false` if confirmed
   - notification: the notification deadline
   - cfp: a link to the call for paper
	- venue: an object with the name of the city, and an additional field with the full address
	- dates: the dates when the conference takes place


## Maintaining `deadlines.json`

Search the web to keep `deadlines.json` current. Do this for **both the current year and the next year** — many conferences publish their CFP for the following year well in advance (e.g. USENIX Security'27 and ASIACCS'27 open submissions a year ahead).

### When to update

- When explicitly asked to refresh deadlines
- When a conference is missing from the database
- Check the following community aggregators first before searching individual conference sites:
  - https://se-deadlines.github.io/ — software engineering
  - https://sec-deadlines.github.io/ — security

### What to add

- Add one entry per track/submission round for each conference instance
- Include conferences for the current year **and** the next calendar year
- For conferences with multiple review cycles, add a separate entry per cycle using the track naming convention: `"research (cycle 1)"`, `"research (cycle 2)"`, etc.
- If a deadline is not yet announced, omit the entry rather than guessing

### What to remove

- Remove entries for conferences for which the **conference itself** has already taken place (not just the deadline)
- Do not remove entries just because the submission deadline has passed — the website filters those out dynamically

### `venue` object

The venue object must include `lat`, `lon`, and `region` in addition to `city` and `address`. Valid `region` values: `"Europe"`, `"Asia"`, `"Middle East"`, `"Australasia"`, `"North America"`, `"other"`.

### Estimated deadlines

If the submission deadline is not yet officially announced but the conference is confirmed, you may add an entry with an estimated deadline based on the previous year's pattern, e.g. ±2 weeks of the same date the prior year. In this case, set `"estimated": true` on the entry. The website will display estimated deadlines with a visual indicator (e.g. a tilde prefix `~`) to inform users they should verify the date.

Do **not** add an estimated entry if the conference itself has not been announced yet.

### Verifying data

Always check the official conference website for the submission deadline — aggregator sites can lag. Prefer the HotCRP submission site or the conf.researchr.org page for exact dates. When you find a confirmed date for an entry marked `"estimated": true`, remove the `estimated` field (or set it to `false`).

### URL validation

All URLs in `conferences.json` and `deadlines.json` must be verified before committing. For each URL:

- Confirm the domain resolves and the page returns HTTP 200
- Set the field to `null` if the page returns 404, ECONNREFUSED, or redirects to an unrelated site
- Do **not** invent or guess URLs — only use URLs you have confirmed by fetching the page

Known issues to watch for:
- Series-level URLs (e.g. for ESORICS, ASIACCS) often do not have a stable domain — use `null` if no reliable series page exists
- CFP track pages on conf.researchr.org (e.g. `/track/fse-2027/…`) are often not yet created for future conferences — set `cfp` to `null` until the page exists
- The domain `www.icsme.org` has been hijacked; use `https://conf.researchr.org/series/ICSME` instead
- USENIX conference CFP sub-pages (e.g. `/conference/usenixsecurity27/call-for-papers`) may not exist yet; use the conference homepage URL only


# The Website

The website loads the databases, and displays the following: 


## Deadline Table

- a table of *upcoming* conference deadlines, ordered by date
- controls to filter table entries by: 
   - field of research (using check boxes)
   - CORE ranking (use a threshold, show all over threshold, include an unranked option)
   - deadline (using sliders)
   - location (using checkboxes with options "Europe", "Asia", "Middle East", "Australasia", "North America", "other".
   - sponsoring information, include a "n/a or other" option, with checkboxes for common options

   
The deadline table should display information, including links, from `deadlines.json` and `conferences.json`.   
 
 
##  Location Map

A map of the world (use an embedded map like Google Maps, use free alternative if possible). Conferences with upcoming deadlines are highlighted. If a venue is selected, the details of the respective venues are displayed in a table below the map, using the format described above but without the filter options. If a user hoovers over a location pin, a brief summary is displayed (acronym and submission deadline, with hint "click for more"). 


## Style

Use a minimalistic design, with the following features: 

- a switch between light and dark theme, cookies can be used to remember use preferences
- use a tabbed layout with tabs for locations (the default) and the deadlines 


# Deployment

Create a static website to test locally first. 
 
  
  
	
	
	
	
   
   





   
   

   
   




in the following fields, sourced from [https://portal.core.edu.au/conf-ranks/](https://portal.core.edu.au/conf-ranks/). Note that by leaving the search field blank, all conferences can be listed.

