# UKR.NET mail integration for Codex, Claude, and Cursor

Status: ready-for-agent

## Problem Statement

People who use UKR.NET mail want to find, read, draft, and send email from Codex, Claude, and Cursor without giving a third-party hosted service their mailbox credentials. They need a clear way to install the integration, connect their account, understand what data is shared with the assistant, and remain in control of actions that send or change mail.

## Solution

Provide a locally executed MCP server shared across Codex, Claude, and Cursor, with the client-specific packaging and configuration needed for each that connects directly to UKR.NET over encrypted IMAP and SMTP. Distribute it through a public, Git-backed community marketplace that users explicitly add. Guide users through creating a dedicated UKR.NET application password and storing it securely on their own computer. Provide focused mail workflows, with an enforceable user approval step before sending. Explain the marketplace installation steps and the limits of local execution and model-visible email data.

## User Stories

1. As a UKR.NET user, I want to discover a public source repository, so that I can inspect the plugin before installing it.
2. As a UKR.NET user, I want clear marketplace installation instructions, so that I can install the plugin in a supported Codex environment.
3. As a UKR.NET user, I want to understand that adding a marketplace or configuring a client is a separate step, so that I know what to expect.
4. As a UKR.NET user, I want supported platforms and prerequisites stated clearly, so that I know whether installation will work for me.
5. As a UKR.NET user, I want versioned releases and update instructions, so that I can make informed decisions about updates.
6. As a UKR.NET user, I want to connect directly from my computer, so that an operator does not receive my mail password.
7. As a UKR.NET user, I want to enable external access and create an application password using UKR.NET, so that I do not disclose my main account password to the plugin.
8. As a UKR.NET user, I want to enter the application password through a secure local setup flow, so that it does not appear in chat or command history.
9. As a UKR.NET user, I want my password stored in the operating system credential store, so that it is not saved in plaintext configuration.
10. As a UKR.NET user, I want actionable setup errors if secure credential storage is unavailable, so that I can resolve the problem without an unsafe fallback.
11. As a UKR.NET user, I want to replace or remove saved credentials, so that I can recover from rotation or disconnect the integration.
12. As a UKR.NET user, I want to know how to revoke the app password at UKR.NET, so that I can invalidate access independently.
13. As a UKR.NET user, I want the server to validate encrypted connections and certificates, so that my credentials and mail are protected in transit.
14. As a UKR.NET user, I want readable authentication and network errors, so that I can fix connection problems without exposing secrets.
15. As a UKR.NET user, I want to list my mail folders, so that I can choose where to search.
16. As a UKR.NET user, I want to search within a folder by common message attributes, so that I can find relevant mail.
17. As a UKR.NET user, I want bounded, paginated search results, so that the assistant does not retrieve my entire mailbox unnecessarily.
18. As a UKR.NET user, I want to inspect headers before opening a message, so that I can choose which content to share.
19. As a UKR.NET user, I want to read a selected message, so that Codex can help me understand it.
20. As a UKR.NET user, I want message identifiers that remain stable across mailbox changes, so that a later request refers to the intended message.
21. As a UKR.NET user, I want a clear explanation that retrieved email can enter the model context, so that I can decide what to access.
22. As a UKR.NET user, I want HTML and oversized content handled safely, so that reading messages does not execute active content or overwhelm the session.
23. As a UKR.NET user, I want untrusted message content treated as data, so that an email cannot authorize instructions or actions on my behalf.
24. As a UKR.NET user, I want to compose a draft for review, so that I can revise a message before it is sent.
25. As a UKR.NET user, I want replies to address the intended recipients and preserve relevant threading headers, so that conversations remain understandable.
26. As a UKR.NET user, I want to review recipients, subject, body and any attachments before sending, so that I control the content and destination.
27. As a UKR.NET user, I want explicit approval bound to the exact message being sent, so that an altered draft cannot reuse my approval.
28. As a UKR.NET user, I want sending to fail closed if the host cannot enforce approval, so that an assistant or malicious email cannot silently send a message.
29. As a UKR.NET user, I want the authenticated account to match the sender, so that messages comply with UKR.NET's requirements.
30. As a UKR.NET user, I want an accurate indication of SMTP acceptance, so that I do not mistake acceptance for final delivery.
31. As a UKR.NET user, I want ambiguous send failures surfaced without automatic resend, so that I can avoid sending duplicates.
32. As a UKR.NET user, I want sent-folder behavior verified, so that my outgoing messages are visible without duplicates.
33. As a UKR.NET user, I want support for Ukrainian and other Unicode text, so that messages display correctly.
34. As a UKR.NET user, I want no email content or credentials in ordinary logs, so that local diagnostics do not create unnecessary exposure.
35. As a UKR.NET user, I want to uninstall the plugin and remove saved credentials, so that I can stop using it cleanly.
36. As a maintainer, I want realistic automated tests without real credentials, so that changes can be validated safely.
37. As a maintainer, I want optional real-account checks, so that provider-specific behavior can be verified before release.
38. As a maintainer, I want a reproducible release and dependency review process, so that users can assess the software they run.
39. As a maintainer, I want honest distribution and approval documentation, so that users do not mistake a community marketplace for the public OpenAI directory.

40. As a Claude user, I want client-specific local installation instructions, so that I can use the integration in supported Claude surfaces.
41. As a Cursor user, I want client-specific local MCP setup instructions, so that I can connect the same server in the editor and supported CLI.
42. As a user of multiple assistants, I want the same mail and security behavior in each supported client, so that switching clients does not weaken my protection.
43. As a user of a cloud or restricted environment, I want clear compatibility limits, so that I know when my local server is unavailable.

44. As a UKR.NET user, I want to list and inspect available drafts, so that I can resume work without accidentally sending.
45. As a UKR.NET user, I want to update only selected draft fields, so that unrelated recipients and content are preserved.
46. As a UKR.NET user, I want to review attachment metadata before requesting its contents, so that I control what enters the model context.
47. As a UKR.NET user, I want any conversation view to identify incomplete or uncertain results, so that I do not mistake an IMAP approximation for a complete thread.

## Implementation Decisions

- Build one host-independent local stdio MCP server with concise workflow instructions and client-specific configuration or packaging for Codex, Claude, and Cursor. Provide documented paths for Codex local marketplace, Claude Code and Claude Desktop local MCP, and Cursor editor and CLI local MCP; verify each surface before claiming support. A Claude Desktop extension package may be evaluated separately; directory listing and one-click installation are not assumed. Hosted/cloud-only sessions may not be able to reach a local server. The plugin communicates directly with UKR.NET; no operator-hosted relay, credential vault, or telemetry service is part of the solution.
- Use a new public GitHub repository under nise-ua as the common source and release location. Distribute the Codex package through a publicly accessible Git-backed marketplace with versioned releases and documented source registration; provide separate client-specific installation instructions for Claude and Cursor. Adding this marketplace is a prerequisite to installation; it does not produce a listing in the universal public directory. Validate the actual install path and supported client surfaces on clean environments before claiming compatibility.
- Use UKR.NET's documented IMAP endpoint on port 993 with implicit TLS and SMTP endpoint on port 465 with TLS, certificate and hostname verification. Use the account's dedicated application password, full account address where required, authenticated SMTP, and an envelope and From address consistent with the account. Verify username and protocol details against a test account. Treat alternate ports as unsupported until their encryption modes are validated.
- Provide an explicit local setup and disconnect flow. Collect secrets outside model-visible conversation, process arguments, and shell history using a masked local prompt or equivalent. Store secrets in the operating system credential store; do not silently fall back to plaintext, logs, manifests, or environment variables. Document supported platforms, credential-store availability, rotation, and revocation. Never request the account's primary password.
- Expose narrow tools for folder listing, bounded search, reading a specific message, composing a draft and sending an approved draft. Message references include mailbox, UID and UIDVALIDITY; reject stale references rather than reading the wrong message. Prefer read-only operations that do not change message flags.
- Provide bounded draft listing, inspection, creation and revision where draft persistence is supported and validated. Define whether drafts are local or synchronized with the IMAP Drafts folder, lifetime, cleanup, concurrency and compatibility before implementation; never present a local-only draft as synchronized. Sparse updates preserve omitted fields and explicitly clear fields only when requested. If safe persistence is not ready, disclose the limitation rather than silently writing sensitive drafts to insecure storage.
- Provide attachment metadata with selected messages; retrieval is a separate explicit request using an unambiguous message and MIME-part reference, with safe limits. Attachment retrieval and sending may be deferred if secure handling is not ready.
- Where provider behavior permits, expose bounded conversation views derived from standard Message-ID, In-Reply-To and References, and label incomplete or uncertain groupings. Do not promise Gmail thread IDs or infer reliable threads from subject alone. Expose folder or unread counts only when supported accurately.
- Supply concise client-portable workflow instructions and precise tool descriptions: identify read versus write effects, valid identifiers, limits, and how to search, inspect, draft and reply. The inspected Gmail package contains no bundled skills; these are new workflow instructions informed by its exposed tool contracts.
- Drafting alone does not send. An enforceable trusted confirmation must present the full recipients including CC and BCC, subject, body, and attachment details, and bind a one-use approval to the exact immutable payload and account. Reject changed, expired, replayed or absent approvals. Do not rely solely on model instructions, tool annotations, or an untrusted tool argument as proof of consent. Establish that the chosen host provides a reliable confirmation mechanism or implement a trusted local confirmation; disable sending on each client or surface where this cannot be enforced, including automatic-approval configurations.
- Limit data returned to the model, paginate results, set conservative content and attachment limits, and render HTML as inert text. Treat all email and attachment content as untrusted data. Do not execute remote resources, scripts, or instructions in messages. Attachments, if enabled, require explicit selection and size limits; initial sending can omit attachment support if safe handling is not ready, with this limitation disclosed.
- Support Unicode and reply threading; treat SMTP acceptance distinctly from delivery. Avoid automatic retries after ambiguous sends. Test the provider's sent-folder behavior before adding any IMAP append and prevent duplicates.
- Keep credentials and message content out of logs and error reports. State clearly that local operation still exposes selected email to the local process and that tool results can enter the model context and be handled under the host's data policies.
- Pin and review dependencies, bundle what is necessary for supported installations, document runtime requirements, and publish a security contact and responsible disclosure process. Do not claim immunity from prompt injection or compromise.

## Testing Decisions

- A good test asserts observable behavior and security guarantees at the highest practical boundary, rather than internal implementation details. The primary seam is the MCP tool interface, with controlled IMAP/SMTP and credential-store substitutes. The user confirmed this boundary.
- Exercise setup, folder listing, search pagination, UID/UIDVALIDITY handling, selective reads, malformed or hostile messages, Unicode, bounded output, TLS failures, authentication failures, credential redaction, draft patch and persistence semantics, attachment selection, bounded or incomplete conversation views, and cleanup through public interfaces. Test a clean installation and MCP connection for each supported Codex, Claude, and Cursor surface on each supported platform. Verify local operation and client-specific approval behavior, including automatic-approval modes.
- Exercise approval and sending end to end: absent, altered, expired and replayed approvals fail; the exact approved message succeeds once; unexpected errors do not trigger automatic duplicate sends. Verify that hostile email cannot itself authorize a send. Test disconnect and credential-store failure with no plaintext fallback.
- Add a separate opt-in integration test using a dedicated UKR.NET account and app password, never committed to the repository. Verify real IMAP and SMTP authentication, certificate handling, sending, Unicode, sender restrictions, revocation and sent-folder behavior. Avoid sending test mail to unrelated recipients.
- This repository currently has no implementation or existing test suite to use as prior art. Choose mature mail and MCP libraries and testing infrastructure during implementation.

## Out of Scope

- Hosted mail proxy, central storage of credentials or mail, analytics, and public OpenAI directory publication or partner approval.
- Guaranteed one-click installation before the user adds the community marketplace.
- Automatically sending, bulk mailing, deleting, moving, or modifying mailbox messages without explicit user authorization. Deletion, move, and flag-management tools are excluded from the initial release.
- Provider OAuth unless independently verified to exist and separately designed; using OAuth to a hosted relay is not required.
- Full mail-client functionality, push notifications, background mailbox indexing, calendar, contacts, and multi-provider support.

## Further Notes

The official UKR.NET and OpenAI sources and unverified assumptions are recorded in the existing research note. The Gmail tool and packaging comparison is recorded in the companion research note; Gmail-specific labels, thread IDs, search syntax and direct-send behavior are not assumed portable. Client integration guidance: [Cursor MCP documentation](https://cursor.com/docs/mcp), [Claude Desktop local MCP](https://support.claude.com/en/articles/10949351-getting-started-with-local-mcp-servers-on-claude-desktop), and [Claude local connector limitations](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp). Validate current provider and client requirements during implementation and before release. Directory-wide distribution of a local MCP plugin remains subject to OpenAI's partner and allowlist requirements, and is a separate future decision. No live account testing has yet been performed.
