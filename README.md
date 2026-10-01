# Catherine Gathoni

### Content, public forms, and a separate administration boundary

[Lawi Mwaura](https://github.com/Lawi-Mwaura) · [Public preview](https://catherine-six.vercel.app)

![Catherine Gathoni public homepage](assets/catherine-gathoni.jpg)

*Actual public preview captured on 1 October 2026. No dashboard records or customer submissions are shown.*

## Problem statement

Public contact and content journeys need validation, while administrative records require privileged access checks. A notification failure should not erase a successfully saved submission.

## Technologies used

TypeScript · Next.js · React · Tailwind CSS · Supabase · Resend · Tiptap · Zod · Sentry

## Engineering scope

A Next.js website with editorial content, public contact and subscription journeys, and administrative workflows. The active application is separated from alternate deployment and design-handoff artifacts in the source repository.

**Technologies:** TypeScript, Next.js, React, Tailwind CSS, Supabase, Resend, Tiptap, Zod, Sentry.

## System design

**Reading the diagram:** blue = interface; green = processing; gold = data; purple = access, lifecycle, or operational control. Arrow labels describe the handoff between components.

```mermaid
flowchart TB
    PUBLIC[Public pages and forms]
    ADMIN[Administrative interface]
    subgraph SERVER[Next.js server · trust boundary]
        VALIDATE[Normalize and validate input]
        GATE[Server-side administrator check]
        EMAIL[Notification service]
        OPS[Operational error reporting]
    end
    DB[(Supabase persistence)]
    PUBLIC -->|Untrusted submission| VALIDATE
    VALIDATE -->|Validated content| DB
    DB -->|After persistence| EMAIL
    EMAIL -->|Separate delivery errors| OPS
    ADMIN -->|Privileged request| GATE
    GATE -->|Authorized bounded query| DB
    classDef client fill:#EAF2FF,stroke:#3564A3,color:#142D4F,stroke-width:2px;
    classDef service fill:#E7F5F0,stroke:#24745C,color:#123E32,stroke-width:2px;
    classDef data fill:#FFF4D6,stroke:#966F20,color:#4D3810,stroke-width:2px;
    classDef control fill:#F2ECFA,stroke:#7653A1,color:#382451,stroke-width:2px;
    class PUBLIC,ADMIN client;
    class VALIDATE,EMAIL service;
    class DB data;
    class GATE,OPS control;
```

*Simplified responsibility map. Internal entities, credentials, and deployment details are omitted.*

| Concern | Implementation reviewed |
| :--- | :--- |
| Untrusted input | Contact handling trims text, normalizes email, and rejects missing or invalid required fields. |
| Email rendering | Submitted text is escaped before insertion into the contact notification’s HTML. |
| Partial failure | Contact persistence is evaluated before notification delivery; notification errors are reported separately. |
| Administrative access | Dashboard message handlers call a server-side administrator check before reading or changing records. |
| Query bounds | Dashboard lists use pagination rather than returning an unrestricted collection. |
| Operational visibility | Error reporting distinguishes submission, notification, and administrative loading failures. |

## Challenges and tradeoffs

**Persistence and notification have different outcomes.** A saved enquiry should remain saved if a notification fails. The contact route records the submission first and handles notification failure separately. Durable retry handling and delivery monitoring are separate operational questions; this overview does not claim guaranteed email delivery.

**Public forms and administrative operations have different trust levels.** An administration screen alone cannot protect data. Its server routes must evaluate administrative access before making privileged queries. The reviewed message handlers put that check ahead of the operation.

**Rich content brings additional responsibilities.** An editor improves authoring, but content rendering, attachments, and preview behavior need explicit validation. The public showcase does not expose the administrative interface or its records.

## Outcomes

- Contact handling normalizes and validates input and escapes submitted text in notification HTML.
- Persistence and notification results are handled separately.
- Reviewed administrative message routes check access before querying or mutating records.
- Paginated lists bound administrative queries.

These are implementation outcomes supported by the reviewed source, not measured production improvements.

## Metrics and evidence

| Measure | Evidence |
| :--- | :--- |
| Interface evidence | Public web interface captured on 1 October 2026. |
| Verification scope | Source responsibilities reviewed; no complete build, live submission, or administration audit performed. |
| Production metrics | No verified traffic, conversion, latency, or reliability figures supplied. |

## Validation scope

This documentation update reviewed source responsibilities and captured the public interface. It did not execute a complete application build, submit live forms, or perform an end-to-end administration or security audit. Further verification should exercise malformed input, notification failure after persistence, and unauthorized administrative requests.

Source remains private. This repository contains only public interface captures and an engineering overview.

[Contact Lawi](mailto:lawimwaura@gmail.com)
