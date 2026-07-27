# PJM Interconnection (pjm)

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
