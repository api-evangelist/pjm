---
name: Query a PJM Data Miner 2 feed with paging
description: >-
  Obtain a PJM Data Miner subscription key, discover a feed's columns from its
  metadata call, then page a date-bounded query to completion — including the
  archived-data rules for the three LMP feeds.
api: PJM Data Miner 2 API
base_url: https://api.pjm.com/api/v1
operations:
  - GET /api/v1/{feed}/metadata
  - GET /api/v1/{feed}
source: >-
  PJM Data Miner API Guide v15 (2026-02-10) sections III, IV, V and VI; PJM Data
  Miner FAQs. No OpenAPI is retrievable anonymously, so every call below is quoted
  from PJM's own guide rather than from a spec.
generated: '2026-07-27'
method: generated
---

# Query a PJM Data Miner 2 feed with paging

## Before you start

You need a subscription key. It is free but not self-serve, and it will take days, not
minutes.

1. Register a PJM Tools username at
   <https://accountmanager.pjm.com/accountmanager/pages/public/new-user.jsf>. Set the
   password within four hours or a Customer Account Manager must reset it.
2. If your company is a PJM member: sign in to PJM Account Manager, open the Account
   Access tab, choose **Request Access** and select **Data Miner API**.
   If it is not a member: email `accountmanager@pjm.com` with the exact statement
   "I confirm that the PJM Data will be used for internal business purposes only",
   your Account Manager user id, and the email address you subscribed with.
3. Wait for CAM approval and the confirmation email, then read the key from
   **View Profile > Your Subscriptions** in PJM Tools.

Two constraints to design around before you build anything:

- One subscription key is bound to one unique email address. Unlike other PJM tools you
  cannot fan a single email address across multiple accounts. System accounts are
  allowed if a valid email address is attached and the CAM provisions it.
- Redistribution of Data Miner data is prohibited without an active PJM membership.
  Internal business use is permitted for non-members; publishing, commercial use and
  derivative works require at minimum an Associate Membership.

## Step 1 — pick the feed short name

Feeds are addressed by short name, not by long name. `data-model/pjm-data-model.yml`
in this repository carries all 79 known short names (e.g. `da_hrl_lmps`,
`rt_fivemin_hrl_lmps`, `gen_by_fuel`, `hrl_load_metered`, `Pnode`). You can also browse
every feed definition anonymously in the Data Miner UI at <https://dataminer2.pjm.com/list>
without a key — do that first to confirm the feed carries what you need.

A 404 means the short name does not exist. A 401 means the short name is real and your
key is missing or wrong. Use that distinction when debugging.

## Step 2 — read the metadata before the data

```
GET https://api.pjm.com/api/v1/{feed}/metadata
Ocp-Apim-Subscription-Key: <key>
```

The metadata call takes no input attributes and returns the feed definition,
publication frequency and column descriptions. For the three archived feeds it also
returns `enableArchiving`, `archiveCutoffDays` and `enableArchiveFiltering` — read
`archiveCutoffDays` here rather than hard-coding 731 or 186.

## Step 3 — issue the first page

```
GET https://api.pjm.com/api/v1/{feed}?rowCount=50000&startRow=1&sort=datetime_beginning_ept&order=Asc&datetime_beginning_ept=1-1-2015 00:00 to 12-31-2015 23:00&fields=datetime_beginning_utc;pnode_id;congestion_price_da
Ocp-Apim-Subscription-Key: <key>
```

Rules the API enforces:

- `rowCount` and `startRow` are **both required** whenever any other parameter is
  present. `startRow` is 1-based. `rowCount` maxes at 50,000.
- The date range format is `mm-dd-yyyy to mm-dd-yyyy` and must be under **366 days**.
  Exceeding it returns `Error: Date range cannot exceed 366 days`.
- `fields` is a CSV list; multi-value filters are semicolon separated
  (`pnode_id=1;3;48579`); an `&` inside an allowed value must be sent as `%26`
  (`BG&E/MIDATL` becomes `BG%26E/MIDATL`).
- Sort on the same field you filter on — EPT with EPT, UTC with UTC — and filter on
  `pnode_id` rather than pnode name. Both are PJM's own performance guidance.
- LMP rows are versioned: `row_is_current=true` for the latest version only, `false`
  for older versions, `all` for everything; `version_nbr` carries the row version.

## Step 4 — page to completion

Read `TotalRows` from the response body, then loop:

```
startRow = startRow + rowCount
```

until `startRow > TotalRows`. The body also carries a `Links` element with the next-page
URL if you prefer to follow it.

If you set `download=true`:

- only the results are returned; `Links`, `SearchSpecification` and `TotalRows` move
  into response headers, with the count in **`X-TotalRows`**;
- a `Content-Disposition` header is added;
- `rowCount` becomes optional, but an unpublished maximum row threshold still applies —
  crossing it returns **HTTP 400**;
- the response is normally gzip encoded. Do not assume it. Inspect `Content-Encoding`
  and decompress only when it says `gzip`; a 2025-09-09 security change stopped some
  responses being compressed.

Add `format=csv` if you want CSV instead of JSON.

## Step 5 — handle the archived window

`rt_hrl_lmps` and `da_hrl_lmps` archive at 731 days; `rt_fivemin_hrl_lmps` archives at
186 days. On the archived side:

- a datetime filter is **mandatory** — omitting it returns
  `A datetime is missing. Please input values for datetime_beginning_ept or datetime_beginning_utc`;
- start and end must fall in the **same UTC calendar year**;
- **no** `sort` or `order` (results come back sorted by `datetime_beginning_utc` ascending);
- filters are limited to dates, `type`, `row_is_current` and `version_nbr` — anything
  else returns `The API request contains invalid attribute(s) for archived data`;
- a request whose range straddles the cutoff returns
  `Date range in the API request spans over archived and standard data` — split it into
  two requests.

Note that `type` is not an allowed filter on Real-Time Five-Minute LMPs on either side
of the cutoff.

## Step 6 — the two-call pattern for five-minute LMPs by zone

Zone filters are disabled on five-minute LMPs because of data volume. PJM's documented
workaround is two calls:

```
GET /api/v1/pnode?rowCount=50000&startRow=1&fields=pnode_id&pnode_subtype=EHV&zone=PSEG&effective_date=to 3-14-2018&termination_date=03-14-2018 to 12/31/9999exact
GET /api/v1/rt_fivemin_hrl_lmps?rowCount=100&startRow=1&datetime_beginning_ept=3/12/2018&pnode_id=52444;52451;52454;52461
```

To list only currently effective pnodes or aggregates, filter
`terminate_date_ept=12-31/9999exact`.

## Operating rules

- **Rate limit:** 600 connections per minute per user. There is no `RateLimit-*` or
  `Retry-After` header and no documented 429 — throttle client-side. PJM reserves the
  right to limit or terminate access of any user impacting system availability.
- **Errors** arrive as `{"errors":[{"field":"...","message":"...","detail":["..."]}]}`,
  not RFC 9457 problem+json. See `errors/pjm-error-catalog.yml`.
- **No idempotency contract** exists — but every operation here is a GET, so retry is
  safe by method.
- **TLS 1.2 minimum.** Since 2025-11-01 PJM's security appliance rejects an HTTP GET
  that carries a request body; make sure your HTTP client is not attaching one.
- **Rehearse on train first.** `https://api-train.pjm.com/api/v1` mirrors production and
  receives each release before production does. See `sandbox/pjm-sandbox.yml`.
- **Expected wall-clock:** PJM's own load test puts a full previous-day five-minute LMP
  pull for all pnodes at ~4.5 minutes with 50 concurrent users, ~12 minutes with 250.
  Budget accordingly; historical queries are slower.
