# Encore

Public radio of the world, as it airs. Two rooms:

- **The Hall**: concerts, opera and jazz that are on now and coming up, plus stations that play classical, culture or jazz around the clock.
- **The Wire**: a countdown to the next news bulletin, the bulletins due in the next hour, flagship news programmes on air now, and a newsroom with every news station.

`index.html` is self-contained. It loads Google Fonts, and it loads hls.js from jsDelivr only when a browser needs it to play an HLS stream.

## Deploy
Push to a new repo (for example `nickcarbone/encore`), then turn on GitHub Pages from the main branch, root folder.

## Data
The station list and weekly schedule are embedded in `index.html`. `stations.json` is a copy of the same data, kept for editing and for the scraper later.

- Slot times are the station's **local** time. Days run from 1 (Monday) to 7 (Sunday) and can be written as ranges, for example `1-5` or `1,3,4,7`.
- A slot can run past midnight: `23:30`–`00:30` works.
- `hourly` entries list exact bulletin start times for each group of days.
- `tier` records how a time was checked: `week`, `day`, `stated` or `format`. A station marked `src: "pending"` has a stream that works but no charted schedule yet.

## Adding the scraper later
If a `schedule.json` file sits next to `index.html`, the page reads it when it loads and replaces `slots` and `hourly` for each station `id` in the file:

```json
{ "checked": "2026-12-01",
  "stations": [ { "id": "bbc-r3", "slots": [ {"d":"1-4","s":"19:30","e":"21:45","t":"Radio 3 in Concert","k":"concert","tier":"week"} ] } ] }
```

A GitHub Action can write this file every night. These sources were reliable and needed no key during the build: BBC schedule pages (JSON-LD), the Sveriges Radio API, the ORF Ö1 API, the Český rozhlas API, the NRK EPG and the Radio France grid pages.

Streams and schedules checked 19 November 2026. Fourteen round-the-clock stations (eleven jazz, three classical) were added and checked on 22 November 2026, and the four SRG SSR streams moved to HTTPS addresses, because the old ones redirected to plain http.
