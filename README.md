# PJM Interconnection (pjm)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

PJM Interconnection is the regional transmission organization (RTO) that operates the largest competitive wholesale electricity market in the United States, coordinating the movement of electricity across all or parts of Delaware, Illinois, Indiana, Kentucky, Maryland, Michigan, New Jersey, North Carolina, Ohio, Pennsylvania, Tennessee, Virginia, West Virginia and the District of Columbia for roughly 67 million people. PJM sits at the wholesale layer of the energy value chain — it has no retail customers, so no Green Button, ESPI or consumer data-sharing obligation applies to it. Its API posture is honestly "rich data, universally gated": PJM publishes a large machine-readable market data catalogue through the Data Miner 2 API at `https://api.pjm.com/api/v1` and an Azure API Management developer portal at `https://apiportal.pjm.com`, but every feed returns HTTP 401 anonymously and a free subscription key is only issued after a PJM Tools account is registered and approved by a Customer Account Manager. Its one genuinely mandated interface is the FERC Order 889 / NAESB WEQ OASIS node, which is verifiably live and standards-conformant at `https://pjmoasis.pjm.com`, though template submission also requires an authenticated PJM SSO session.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/pjm/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/pjm/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Energy Markets
- Electricity
- Grid
- System Operator
- Wholesale Electricity
- Transmission
- Market Data
- Demand Response

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### PJM Data Miner 2 API

PJM's public wholesale market and system data catalogue, exposed as a REST API behind Azure API Management. Feeds include day-ahead and real-time hourly and five-minute LMPs, ancillary service prices and market results, generation by fuel type, hourly metered/estimated/preliminary load, load forecasts, generation and transmission outages, FTR auction bids and zonal LMPs, pnode and aggregate definitions, and uplift and billing determinants. Each feed supports a `/metadata` call and a search call with `rowCount`/`startRow` paging, field selection, sorting and date-range filters, and JSON or CSV output. The data is designated public by PJM and the key is free, but anonymous calls are rejected — verified HTTP 401 on `GET https://api.pjm.com/api/v1/gen_by_fuel` on 2026-07-27 — and redistribution requires at minimum a PJM Associate Membership.

- **Human URL:** [https://www.pjm.com/markets-and-operations/etools/data-miner-2](https://www.pjm.com/markets-and-operations/etools/data-miner-2)
- **Base URL:** `https://api.pjm.com/api/v1`

#### Tags

- Market Data
- Electricity
- Energy Markets
- Locational Marginal Pricing
- Load
- Outages

#### Properties

- [Documentation](https://www.pjm.com/markets-and-operations/etools/data-miner-2)
- [API Portal](https://apiportal.pjm.com/)
- [Guide](https://www.pjm.com/-/media/DotCom/etools/data-miner-2/data-miner-2-api-guide.ashx)
- [Swagger UI](https://services.pjm.com/PJMDataminerApi/swagger)
- [Console](https://dataminer2.pjm.com/list)
- [Sign Up](https://accountmanager.pjm.com/accountmanager/pages/public/new-user.jsf)

### PJM OASIS Template API

PJM's Open Access Same-Time Information System (OASIS) node, the FERC Order 889 obligation implemented against the NAESB Wholesale Electric Quadrant business practice standards. PJM states support for NAESB WEQ-002 (Standards and Communication Protocol) and WEQ-003 (Data Dictionary) versions 2.0, 2.1, 2.2 and 3.3, accepting query/response and input/response templates over HTTP GET (`Content-type: text/plain`) and POST (`application/x-www-form-urlencoded`) against the template base URL, plus three PJM custom templates (`pjmannulment`, `pjmtransreq`, `pjmtsrcomment`). The NAESB WEQ-002-4.5.2 public information page at `https://pjmoasis.pjm.com/OASIS/PJM/INFO.HTM` is served anonymously (verified HTTP 200 on 2026-07-27); template submission itself requires a PJM SSO session token or the PJM Command Line Interface tool.

- **Human URL:** [https://www.pjm.com/markets-and-operations/etools/oasis.aspx](https://www.pjm.com/markets-and-operations/etools/oasis.aspx)
- **Base URL:** `https://pjmoasis.pjm.com/OASIS/PJM/data/`

#### Tags

- OASIS
- Transmission
- NAESB
- FERC Order 889
- Available Transfer Capability

#### Properties

- [Documentation](https://www.pjm.com/markets-and-operations/etools/oasis.aspx)
- [Guide](https://www.pjm.com/-/media/DotCom/etools/oasis/pjm-oasis-api-user-guide.pdf)
- [Public Information](https://pjmoasis.pjm.com/OASIS/PJM/INFO.HTM)
- [Sandbox](https://oasistrain.pjm.com/)
- [CLI](https://www.pjm.com/-/media/DotCom/etools/pjm-command-line-interface-java-11.zip)
- [Standard](https://www.naesb.org/weq/default.asp)

### PJM Browserless Authentication API

The REST authentication front door for every browserless/API integration with PJM eTools. A client POSTs to the PJM single sign-on service with `X-OpenAM-Username` and `X-OpenAM-Password` headers and receives a JSON `tokenId`, which is then presented as a `pjmauth` cookie on subsequent tool API calls, and released via a logout call. PJM has additionally moved custom-code REST clients onto PKI, requiring a two-way TLS client certificate against the `access/authenticate/pjmauthcert` endpoint alongside credentials. The service is ForgeRock OpenAM — verified live with HTTP 405 on `GET https://sso.pjm.com/access/authenticate/` on 2026-07-27, confirming the documented POST-only endpoint.

- **Human URL:** [https://www.pjm.com/markets-and-operations/etools/security.aspx](https://www.pjm.com/markets-and-operations/etools/security.aspx)
- **Base URL:** `https://sso.pjm.com/access`

#### Tags

- Authentication
- Single Sign-On
- Security
- PKI

#### Properties

- [Guide](https://www.pjm.com/-/media/DotCom/etools/pjm-browserless-authentication-guide.pdf)
- [Documentation](https://www.pjm.com/markets-and-operations/etools/security.aspx)
- [Guide](https://www.pjm.com/-/media/DotCom/etools/security/pki-authentication-guide.pdf)
- [Sandbox](https://ssotrain.pjm.com/)

### PJM InSchedule Browserless API

The browserless REST interface to PJM InSchedule, the eTool market participants use to submit and retrieve bilateral internal energy transaction schedules and contracts. PJM's browserless authentication guide documents CSV file upload under `/inschedule/rest/secure/upload/file/{filename}` and CSV retrieval under `/inschedule/rest/secure/download/csv/contracts` with `start` and `stop` date parameters, both authenticated with a `pjmauth` session cookie. Anonymous access is refused — an unauthenticated GET of the documented contracts endpoint on 2026-07-27 returned the ForgeRock OpenAM sign-in page rather than data. This is a member/participant surface, not an open data API.

- **Human URL:** [https://www.pjm.com/markets-and-operations/etools/inschedule.aspx](https://www.pjm.com/markets-and-operations/etools/inschedule.aspx)
- **Base URL:** `https://insched.pjm.com/inschedule/rest`

#### Tags

- Scheduling
- Bilateral Transactions
- Energy Markets
- Members Only

#### Properties

- [Guide](https://www.pjm.com/-/media/DotCom/etools/pjm-browserless-authentication-guide.pdf)
- [Sandbox](https://inschedtrain.pjm.com/)

### PJM eDART Browserless API

The browserless XML-over-HTTP interface to eDART, PJM's Dispatcher Applications and Reporting Tool, used by transmission owners and generation owners to file and query transmission and generator outage tickets, voltage schedules and other reliability data. PJM publishes the XML document definitions for the general endpoints openly at `https://www.pjm.com/pjmfiles/pub/etools/edart/xmldocs/xmldoc.html` (verified HTTP 200, ~482KB, on 2026-07-27), while the endpoints themselves sit behind PJM SSO and PKI at `https://edartsso.pjm.com/edart/`. Documentation is public; the data is participant-only.

- **Human URL:** [https://www.pjm.com/markets-and-operations/etools/edart.aspx](https://www.pjm.com/markets-and-operations/etools/edart.aspx)
- **Base URL:** `https://edartsso.pjm.com/edart/`

#### Tags

- Outages
- Reliability
- Transmission
- XML
- Members Only

#### Properties

- [Documentation](https://www.pjm.com/pjmfiles/pub/etools/edart/xmldocs/xmldoc.html)
- [Guide](https://www.pjm.com/-/media/DotCom/etools/edart/dart-browserless-user-guide.pdf)
- [Sandbox](https://edartssotrain.pjm.com/edartsso)

## Common Properties

- [Website](https://www.pjm.com/)
- [API Portal](https://apiportal.pjm.com/)
- [Documentation](https://www.pjm.com/markets-and-operations/etools)
- [Sign Up](https://accountmanager.pjm.com/accountmanager/pages/public/new-user.jsf)
- [Authentication](https://www.pjm.com/-/media/DotCom/etools/pjm-browserless-authentication-guide.pdf)
- [Security](https://www.pjm.com/markets-and-operations/etools/security.aspx)
- [Blog](https://insidelines.pjm.com/)
- [Learning](https://learn.pjm.com/)
- [Support](https://pjm.my.site.com/publicknowledge/s/)
- [LinkedIn](https://www.linkedin.com/company/pjm-interconnection)
- [Legal](https://www.pjm.com/about-pjm/legal)

## Mandate and Access Posture

- **Home market:** United States
- **Mandate regime:** `other` — FERC Order 889 / NAESB WEQ OASIS. No consumer energy data right (Green Button, CDR, Ontario regulation) applies to a wholesale RTO.
- **Mandate status:** `live-implemented` — verified by the anonymously reachable NAESB WEQ-002-4.5.2 public posting at `https://pjmoasis.pjm.com/OASIS/PJM/INFO.HTM` (HTTP 200) and PJM's own OASIS API User Guide declaring WEQ-002/003 versions 2.0–3.3 support with a published template base URL.
- **Data standard:** NAESB WEQ-002 / WEQ-003 for OASIS; proprietary feed shape for Data Miner 2; no Green Button / ESPI reference found.
- **Consumer data API:** none — PJM has no retail customers.
- **Market data open:** no — the market data is deep and free of charge but key-gated; anonymous calls to `https://api.pjm.com/api/v1/*` return HTTP 401, and redistribution requires PJM membership.
- **Access gate:** application-approval — register a PJM Tools account, request Data Miner API access (members) or email `accountmanager@pjm.com` attesting internal-business-only use (non-members), wait for Customer Account Manager approval, then read the subscription key from View Profile.
- **Auth model:** Azure APIM subscription key (`Ocp-Apim-Subscription-Key`) for Data Miner 2; ForgeRock OpenAM SSO token / `pjmauth` cookie for OASIS, InSchedule and eDART; mTLS client certificate for custom REST clients. No OpenID Connect discovery document is published.

## Artifacts

Enrichment round 2026-07-27. Everything below is either a document fetched from PJM, a
faithful capture of PJM's own published guides, or an honest derivation from a verified
live response. Nothing is invented.

- [authentication/pjm-authentication.yml](authentication/pjm-authentication.yml) — every auth scheme across the six surfaces: APIM subscription key (header and query), ForgeRock OpenAM session token, PKI/mTLS, and anonymous.
- [conventions/pjm-conventions.yml](conventions/pjm-conventions.yml) — paging (`rowCount`/`startRow`, `X-TotalRows`), filtering, content negotiation, error envelopes, versioning. Idempotency is recorded as **absent**.
- [errors/pjm-error-catalog.yml](errors/pjm-error-catalog.yml) — the seven documented validation errors verbatim, plus every HTTP status observed live.
- [rate-limits/pjm-rate-limits.yml](rate-limits/pjm-rate-limits.yml) — 600 connections/minute, 50,000 rows/fetch, 366-day range cap, redistribution restriction.
- [lifecycle/pjm-lifecycle.yml](lifecycle/pjm-lifecycle.yml) — versioning, archive cutoffs (731/186 days), retired feeds and columns, status surface, roadmap. No SLA, no Sunset headers.
- [changelog/pjm-changelog.yml](changelog/pjm-changelog.yml) — Data Miner releases 22.04 → 25.11, structured, plus the API Guide revision history.
- [sandbox/pjm-sandbox.yml](sandbox/pjm-sandbox.yml) — the five parallel `*train.pjm.com` environments and the `pjmauthtrain` cookie gotcha.
- [data-model/pjm-data-model.yml](data-model/pjm-data-model.yml) — 79 Data Miner feed short names and the pnode / aggregate / tie-line entity map.
- [conformance/pjm-conformance.yml](conformance/pjm-conformance.yml) — NAESB WEQ-002/003, FERC Order 889, TLS 1.2, RFC 9116 — and explicit non-conformance for OAuth2, OIDC, RFC 9457, RFC 8594 and AsyncAPI.
- [vocabulary/pjm-vocabulary.yml](vocabulary/pjm-vocabulary.yml) — PJM Glossary, the feed dictionary, the eDART XML docs, the 2022 reserve-name crosswalk.
- [packages/pjm-packages.yml](packages/pjm-packages.yml) — no SDK in any registry, no GitHub org; the Java 11 CLI is the only first-party distributable.
- [cli/pjm-cli.yml](cli/pjm-cli.yml) — the PJM Command Line Interface for OASIS templates.
- [well-known/](well-known/) — PJM's real RFC 9116 `security.txt` plus the full probe matrix across six hosts.
- [security/](security/) — vulnerability disclosure programme (`securityVDP@pjm.com`), TLS/DNS/SPF/DMARC posture.
- [examples/](examples/) and [json-schema/pjm-message.json](json-schema/pjm-message.json) — verbatim Messages web service responses (JSON and XML) and a schema derived from them.
- [skills/](skills/) — three packaged agent skills: query a Data Miner feed with paging, watch the tool change notices, authenticate a browserless eTools client.
- [llms/pjm-llms.txt](llms/pjm-llms.txt) — generated; PJM publishes no `llms.txt`.

## Notes

**No OpenAPI, AsyncAPI, GraphQL or Postman artifact is stored in this repository, and none was fabricated.** A real Data Miner spec exists — PJM's API Guide says "The API definition is also available in Swagger and WADL formats" inside the API Management portal — but it cannot be retrieved anonymously. Re-verified 2026-07-27: the Swashbuckle discovery document at `https://services.pjm.com/PJMDataminerApi/swagger/docs/v1` still returns HTTP 500, every alternate Swashbuckle path returns 404, and the Azure APIM developer data plane at `https://apiportal.pjm.com/developer/apis?api-version=2022-04-01-preview` returns `{"value":[],"nextLink":null}` — it answers anonymously but publishes nothing to an unauthenticated caller.

One correction to the first round: PJM **does** operate a fully anonymous public REST API. `GET https://messages.pjm.com/messages/rest/public/messages` returns HTTP 200 with no credential, serving XML by default and JSON under `Accept: application/json`. It is linked as "Web Service" from the Upcoming Changes page and carries planned-outage and impact notices for the PJM tool estate. It is now catalogued as the sixth API here, with a verbatim example capture and a derived JSON Schema.

Also confirmed absent: no official SDK on any package registry, no GitHub organisation, no public Postman collection, no MCP server, no webhooks or event stream, and no uptime status page or SLA.

See [review.yml](review.yml) for every URL probed and its HTTP status.
