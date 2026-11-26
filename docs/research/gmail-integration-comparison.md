# Gmail integration comparison

Reviewed 2026-11-26 against the Gmail connector tool descriptions available in this session and the locally installed Gmail plugin package, version 0.1.10. This is an interface review, not an examination of its server implementation or a test of a Gmail account.

The installed package's README says it searches mail, reads threads, drafts, organizes and triages; it declares that it contains no bundled skills or skill-specific helper scripts. Its manifest registers a connected app. We therefore found no Gmail skill to adopt or audit. Tool descriptions can guide behavior; they do not establish how Gmail implements security or provide reusable source code.

## Patterns to adapt

- Separate bounded, paginated search from opening selected messages. Offer header-only reads and optional content reads, and explicit, validated message references. IMAP identities must use mailbox, UIDVALIDITY and UID rather than Gmail message IDs.
- Expose folder counts when IMAP/provider capabilities reliably provide them, and distinguish unread counts from search results. Do not claim Gmail label semantics.
- Provide focused draft creation, listing, inspection, revision and sending, with clear patch semantics: omitted fields remain unchanged; explicitly cleared fields are cleared. Specify persistence and provider compatibility before implementing shared drafts. Preparing a draft is distinct from sending, and sending continues to require the spec's enforceable, exact-payload, one-use user approval.
- Read attachments only on explicit request, after returning metadata and an unambiguous parent message/part reference. Apply strict size and content handling limits; do not execute files or fetch remote content. Attachment support is contingent on safe implementation and may be deferred.
- Consider bounded conversation views that use RFC Message-ID, In-Reply-To and References when supported. IMAP does not inherently provide Gmail thread IDs or guarantee complete threading; communicate incomplete results and do not group solely by subject.
- Write focused tool descriptions that distinguish reads from changes, identify valid input identifiers, define result limits, and tell the agent to avoid guessing identifiers. Provide brief host-portable workflow guidance for search, read, draft and reply without relying on instructions as a security boundary.

## Deliberately not copied

Gmail labels, archive, bulk mutation, trash and provider-specific search syntax are not portable IMAP equivalents and are outside the initial release. The Gmail tool contracts also include direct send and forwarding, HTML-first composition, large batch limits, and attachment handling; these are not requirements to duplicate. The UKR.NET integration keeps its stronger send-approval requirement, conservative limits, inert message rendering and no automatic forwarding. Provider OAuth and Gmail connector authentication do not establish UKR.NET OAuth support.
