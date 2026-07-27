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

## Notes

No OpenAPI, AsyncAPI, Postman or JSON Schema artifact is stored in this repository. The only machine-readable contract endpoint discovered — the Swashbuckle discovery document at `https://services.pjm.com/PJMDataminerApi/swagger/docs/v1`, declared by the publicly reachable Swagger UI at `https://services.pjm.com/PJMDataminerApi/swagger` — returned HTTP 500 on 2026-07-27. Nothing was generated to fill the gap. See [review.yml](review.yml) for every URL probed and its HTTP status.
