---
name: Authenticate a browserless client to PJM eTools
description: >-
  Obtain a PJM single sign-on token over the ForgeRock OpenAM REST endpoint, present it
  as the pjmauth cookie against an eTool browserless interface, and release it — the
  prerequisite for InSchedule, eDART and OASIS template automation.
api: PJM Browserless Authentication API
base_url: https://sso.pjm.com/access
operations:
  - POST /access/authenticate/
  - POST /access/logout/
  - POST /access/authenticate/pjmauthcert
source: >-
  PJM Browserless Authentication Guide
  (https://www.pjm.com/-/media/DotCom/etools/pjm-browserless-authentication-guide.pdf),
  PJM PKI-Based Authentication Guide, PJM OASIS API User Guide Rev 04, PJM eDART
  Browserless User Guide v1.28.0. Endpoint liveness verified 2026-07-27.
generated: '2026-07-27'
method: generated
---

# Authenticate a browserless client to PJM eTools

Every PJM eTool other than Data Miner 2 is gated by a ForgeRock OpenAM session rather
than an API key. This is the handshake all of them share.

## Prerequisites

- A PJM Tools username provisioned for the specific tool you intend to call. Access is
  granted per tool by your company's Customer Account Manager — a working account for
  InSchedule does not imply access to eDART.
- For custom-code REST clients, a PJM-issued PKI client certificate. PJM has moved
  custom-code clients onto two-way TLS; you present the certificate against the
  `access/authenticate/pjmauthcert` endpoint alongside credentials.

## Step 1 — authenticate

```
POST https://sso.pjm.com/access/authenticate/
X-OpenAM-Username: <username>
X-OpenAM-Password: <password>
Content-Type: application/json
```

The JSON response body carries a `tokenId`. `GET` on this endpoint returns **HTTP 405
Method Not Allowed** — verified 2026-07-27 — so if you are seeing a 405 your client is
using the wrong method, not the wrong credentials.

For a PKI client, target `access/authenticate/pjmauthcert` over a mutually authenticated
TLS connection with your client certificate presented, using the same credential
headers.

## Step 2 — present the token

Send the `tokenId` as a cookie on every subsequent call to the tool API:

```
Cookie: pjmauth=<tokenId>
```

On the training environments the cookie is named **`pjmauthtrain`**, not `pjmauth`. This
is the single most common cause of a train integration silently receiving the OpenAM
sign-in page instead of data — pointing at the train hostname is not enough, you must
rename the cookie too.

## Step 3 — call the tool

Examples PJM documents:

- **InSchedule** — CSV upload to
  `https://insched.pjm.com/inschedule/rest/secure/upload/file/{filename}`, CSV retrieval
  from `https://insched.pjm.com/inschedule/rest/secure/download/csv/contracts` with
  start and stop date parameters.
- **eDART** — XML over HTTP against `https://edartsso.pjm.com/edart/`. The XML document
  definitions are published openly at
  <https://www.pjm.com/pjmfiles/pub/etools/edart/xmldocs/xmldoc.html> even though the
  endpoints are participant-only.
- **OASIS** — NAESB WEQ-002 template submission against
  `https://pjmoasis.pjm.com/OASIS/PJM/data/`, GET with `Content-type: text/plain` or
  POST with `Content-type: application/x-www-form-urlencoded`, per WEQ-002-4.2.4
  through 4.2.7. PJM also ships a first-party Java 11 command line client for this —
  see `cli/pjm-cli.yml`.

## Step 4 — release the session

```
POST https://sso.pjm.com/access/logout/
```

Do not leak sessions across scheduled runs. Authenticate, do the work, log out.

## Failure modes to code for

- **An HTTP 200 that is HTML.** An unauthenticated or expired session against a secure
  eTool path returns the ForgeRock OpenAM sign-in page with a 200 status, not a 401.
  Verified: an anonymous GET of the documented InSchedule contracts endpoint on
  2026-07-27 returned the CDDL-headered OpenAM login HTML. Check the content type and
  body, never the status code alone.
- **RESTEasy error documents.** eTools JAX-RS surfaces return
  `<errorResponse><errorType>...</errorType><message>...</message></errorResponse>` as
  XML, sometimes with a 500 status for what is semantically a 404.
- **TLS.** TLS 1.2 is the floor across all PJM tools. Since 2025-11-01 PJM's security
  appliance enforces strict HTTP rules — an HTTP GET carrying a body is rejected.

## Rehearse on train

Every surface has a parallel training host: `ssotrain.pjm.com`, `inschedtrain.pjm.com`,
`edartssotrain.pjm.com`, `oasistrain.pjm.com`, `api-train.pjm.com`. Releases land there
first. Full map in `sandbox/pjm-sandbox.yml`.
