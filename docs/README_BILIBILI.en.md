# usable-search-engine (Bilibili edition)

[中文](README.md) · English

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![dependencies](https://img.shields.io/badge/dependencies-0-brightgreen)
![license](https://img.shields.io/badge/license-MIT-blue)
![platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)
![version](https://img.shields.io/badge/version-v0.1.0-orange)

## Fighting the algorithm

A locally-run search tool for Bilibili with its own ranking. Six search dimensions: everything / title / description / tag / uploader / UID.

The results page is mostly recommendations — trending, "related", promoted — with a handful of real hits mixed in. Keyword matches get buried under fresh re-uploads and clips, searching an uploader's name returns videos that merely contain those characters, and descriptions or tags are not treated as searchable at all. Filtering and sorting are limited to the few presets the platform offers.

Measured: searching a song title scanned 100 results, of which only 48 actually had the keyword in the title — over half did not contain it where it counts.

This project pulls the results out of that pile and ranks them by rules you choose: relevance, where the keyword hit, play count, publish date.

Runs entirely on your machine, listens on `127.0.0.1:18788`, uploads nothing, needs no account.

## What you get

- **Six dimensions** — everything / title / description / tag / uploader / UID
- **Filters** — duration (10–15 / 15–30 / 30–60 / 60–120 min / 2 h+, or custom minutes), publish date (today … older than a year, or custom range)
- **Sorting** — relevance, heat, newest, oldest, play count, danmaku, favourites
- **Match strictness** — standard (title/description/tag) · standard without tags · strict (title only) · loose (platform order)
- **Hit visibility** — every card says whether it matched on title, description or tag
- **Local archive** — every video that passes the filters and gets shown is recorded; one click re-checks each one and classifies it as alive / deleted / private / unknown (unknown is never reported as deleted)
- **Backup-site lookup** — off by default; HEAD requests only, one per second, serial, cached forever on hit
- **Probe log** — success rate, p50, p95 per request path

## Run it

```
git clone https://github.com/kipp546IO/usable-search-engine-BILI.git
cd usable-search-engine-BILI
python -m venv .venv
.venv\Scripts\activate          # Windows; source .venv/bin/activate on macOS/Linux
pip install -e .
python -m usable_search serve
```

Then open <http://127.0.0.1:18788/>. On Windows you can just double-click `install.cmd`.

Pure Python standard library only — no third-party packages. Data lives in `data/` (gitignored). The service binds to localhost only.

## Known limits

- Uploader video lists are available from each uploader card; that endpoint is behind Bilibili's risk control and sometimes answers with -352, in which case you get a "try later" message.
- First search per keyword scans several pages and can take a few seconds; repeated queries hit the local cache for 10 minutes.
- Only public metadata is fetched. No account and no login; the only cookie sent is an anonymous device id (buvid3). No video content.

MIT licensed.
