---
name: Watch PJM tool change and outage notices
description: >-
  Poll PJM's only anonymous public REST service for planned outages, delayed data and
  impact notices across the PJM eTools estate, and turn the payload into scheduled
  change awareness for an integration.
api: PJM Messages Public Web Service
base_url: https://messages.pjm.com/messages/rest/public
operations:
  - GET /messages/rest/public/messages
source: >-
  Linked as "Web Service" from
  https://www.pjm.com/markets-and-operations/etools/upcoming-changes; response verified
  HTTP 200 anonymously on 2026-07-27 and captured verbatim at
  examples/pjm-messages-public-response.json.
generated: '2026-07-27'
method: generated
---

# Watch PJM tool change and outage notices

This is the one PJM API you can call today with no credential, no registration and no
membership. It backs the Upcoming Changes page — "a list of planned outages to PJM
tools, websites and apps, as well as delayed data or report deadlines" — and it is the
right heartbeat for any integration that depends on Data Miner, Markets Gateway,
InSchedule, eDART or OASIS.

## Step 1 — call it

```
GET https://messages.pjm.com/messages/rest/public/messages
Accept: application/json
```

- No authentication of any kind. Verified HTTP 200 anonymously.
- Without an `Accept` header you get `application/xml;charset=UTF-8` with a
  `ns2:messageList` root in namespace `http://esuite.pjm.com/`. Send
  `Accept: application/json` if you want JSON.
- There are no documented query parameters, no paging and no filtering. The service
  returns the currently effective notice set — one small document.

## Step 2 — read the payload

Schema derived from the live response is at `json-schema/pjm-message.json`; a verbatim
capture is at `examples/pjm-messages-public-response.json`. Each entry in `messages[]`
carries:

| field | meaning |
|---|---|
| `userMessageId` | PJM's identifier for the notice — use it to dedupe across polls |
| `subject` | short title |
| `messageRaw` | notice body as authored HTML |
| `message` | the same body pre-wrapped with PJM's display styling |
| `applications` / `applicationIds` | affected PJM tools, by name and by numeric id |
| `effectiveDate` / `terminateDate` | epoch **milliseconds** |
| `changeStart` / `changeEnd` | epoch milliseconds for the change window |
| `changeDuration` | duration of impact (null in the observed capture) |
| `messageActions` | e.g. "No action required/information only", "Please share within your organization" |
| `messageImpacts` | impact descriptors (empty in the observed capture) |
| `appearanceFlag` | whether the notice is currently displayed |
| `blockAccessFlag` | whether the notice corresponds to an access-blocking event |

All four timestamps are epoch milliseconds, not ISO strings. Divide by 1000 before
handing them to a seconds-based date library.

## Step 3 — turn it into signal

1. Poll on a schedule you choose — no rate limit is published for this endpoint, so be
   conservative (hourly is ample for a planned-change feed).
2. Key on `userMessageId` and keep a seen-set; the service returns current notices, not
   a delta.
3. Filter `applications` against the PJM tools your integration actually touches.
4. Alert hardest on `blockAccessFlag: true` and on any notice whose `changeStart` falls
   inside your next batch window.
5. Treat `messageRaw` as untrusted HTML — sanitise before rendering it anywhere.

## What this endpoint is not

- It is **not** a webhook. There is no callback registration anywhere in PJM's surface;
  this is poll-only. The alternative PJM offers is email subscription via
  <https://www.pjm.com/mypjm/newsletters>.
- It is **not** an uptime or incident status page. PJM publishes no component health
  board, no historical uptime and no SLA. This feed covers *planned* change and known
  data delays only — an unplanned outage may never appear here.
- It carries **no market data**. Everything priced, metered or scheduled lives behind the
  Data Miner key (see `skills/pjm-query-data-miner-feed.md`).

## Related

- Human page: <https://www.pjm.com/markets-and-operations/etools/upcoming-changes>
- How-to guide (PDF): <https://www.pjm.com/-/media/etools/upcoming-changes-how-to-guide.pdf>
- Release-by-release detail for Data Miner specifically: `changelog/pjm-changelog.yml`
- Deprecation and archived-data behaviour: `lifecycle/pjm-lifecycle.yml`
