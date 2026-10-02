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

**Component architecture.** The boxes identify technologies and responsibilities; boundaries group the application runtime and managed backend. Relationships show dependencies and integration protocols, rather than a step-by-step processing flow.

```mermaid
C4Component
    title Catherine Gathoni - application architecture
    Container_Boundary(app, "Next.js application") {
        Component(web, "Public and content interface", "React + Tailwind + Tiptap", "Content and public forms")
        Component(api, "Server routes", "Next.js + TypeScript", "Validation and contact handling")
        Component(admin, "Administration boundary", "Server-side access checks", "Privileged message operations")
    }
    System_Boundary(supabase, "Supabase backend platform") {
        Container(auth, "Supabase Auth", "Managed authentication", "Identity checked by server code")
        ContainerDb(db, "Supabase Database", "PostgreSQL", "Application and contact records")
    }
    System_Ext(email, "Resend", "Notification delivery")
    Rel(web, api, "Calls", "HTTPS")
    Rel(api, db, "Persists", "Supabase SDK")
    Rel(api, email, "Notifies", "Resend API")
    Rel(admin, auth, "Checks identity", "Supabase SDK")
    Rel(admin, db, "Runs authorized queries", "Supabase SDK")
    UpdateElementStyle(web, $bgColor="#24486B", $fontColor="#FFFFFF", $borderColor="#24486B")
    UpdateElementStyle(api, $bgColor="#24745C", $fontColor="#FFFFFF", $borderColor="#24745C")
    UpdateElementStyle(auth, $bgColor="#24745C", $fontColor="#FFFFFF", $borderColor="#24745C")
    UpdateElementStyle(admin, $bgColor="#7653A1", $fontColor="#FFFFFF", $borderColor="#7653A1")
    UpdateElementStyle(email, $bgColor="#7653A1", $fontColor="#FFFFFF", $borderColor="#7653A1")
    UpdateElementStyle(db, $bgColor="#966F20", $fontColor="#FFFFFF", $borderColor="#966F20")
    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
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
