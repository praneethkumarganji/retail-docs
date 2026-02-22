# Social — Features (Detailed)

This document explains each Social feature in depth, including basics, UX expectations, APIs, and examples.

1) Channel Linking & Management
- Purpose: connect tenant social accounts (Instagram, Facebook, YouTube, TikTok, LinkedIn) so the platform can publish and receive events.
- Basics:
  - Use OAuth 2.0 or platform-provided token exchange flows.
  - Request minimal scopes required (publish, read_insights, manage_comments).
  - Store credentials encrypted (Vault); never expose raw tokens to client.
- UX:
  - "Connect Channel" button → start OAuth → show required scopes and example post preview.
  - Health panel shows connection status, last sync, rate-limit warnings.
- API examples:
  - POST /bff/social/channels/link -> returns redirect URL for OAuth.
  - GET /bff/social/channels -> lists connected channels with id, name, scopes, status.

2) Composer (Channel-aware)
- Purpose: create posts for one or more channels, including reels and videos.
- Basics:
  - Provide fields: text body, media attachments, scheduled time, target channels, product tags, CTA.
  - Enforce channel constraints client-side and server-side (max length, video duration, aspect ratios).
- UX:
  - Channel selector shows channel-specific preview and validation messages inline.
  - Templates and A/B variants selector.
  - "Save Draft" and "Schedule" actions.
- Implementation notes:
  - Validate on client for immediate feedback; re-validate on server before publishing.
  - Use resumable uploads for large media; return upload id to composer.

3) Publishing & Delivery
- Purpose: reliably deliver posts to platforms with retry, backoff, and reporting.
- Basics:
  - Publishing is asynchronous. BFF returns an acceptance id; background workers perform the actual publish.
  - Track publish status via events (published, failed, pending).
- Reliability:
  - Implement exponential backoff for transient errors.
  - Surface actionable failure reasons to UI (quota, permissions, invalid media).
- API patterns:
  - POST /bff/social/posts -> returns postId and queue status.
  - GET /bff/social/posts/:id/status -> returns per-channel status and error details.

4) Scheduling & Queue
- Purpose: schedule posts with time-zone awareness and provide queue management.
- Basics:
  - Support one-off schedules and recurrence rules (cron-like or simplified recurrence).
  - Allow queue reordering and bulk reschedule.
- UX:
  - Calendar view with drag-and-drop to change schedule.
  - Queue shows priority and reason for delays.

5) Asset Library & Media Management
- Purpose: centralize media assets with versions and reuse.
- Basics:
  - Uploads use resumable protocol (tus or multipart + chunks).
  - Store metadata (mime, duration, renditions).
  - Perform server-side transcode for channel requirements and store multiple renditions.
- UX:
  - Asset browser with filters, usage counts, and favorites.
  - Drag into composer to attach.

6) Inbox, Moderation & Intelligent Responses
- Purpose: collect comments, replies, and direct messages into a unified conversation workspace.
- Ingestion:
  - Webhooks from platforms → Webhook Controller → Event Bus → Conversation Service.
  - Normalize incoming events into a common message schema.
- Conversation model:
  - Thread id, messages (from/to), participants, status, assignee, tags, SLA timestamps.
- Moderation:
  - Rules engine for auto-hide, block, escalate.
  - Bulk moderation actions and canned responses.
- Intelligent responses:
  - Intent detection pipeline (classifier + heuristics).
  - Suggest replies with confidence score; allow agent edit before send.
  - Auto-reply for high-confidence, low-risk intents (configurable per-tenant).
- APIs:
  - GET /bff/social/conversations?status=open
  - POST /bff/social/conversations/:id/reply { message, autoReply=false }

7) Conversation Routing & Omni-Channel Support
- Purpose: route conversation items into tenant support queues and integrate with CRM for context.
- Routing:
  - Rules: keyword-based, intent-based, SLA-based routing.
  - Route to support agents, bot, or create CRM leads.
- Omni-Channel:
  - Map conversations across channels to the same customer when possible (email/phone/username).
  - Store external channel ids and cross-reference with CRM profiles.

8) Analytics & Attribution
- Purpose: measure post performance and attribute conversions to social activity.
- Basics:
  - Collect impressions, engagements, clicks, conversions, and revenue per post and channel.
  - Link clicks to orders via UTM parameters or deep links.
- Reporting:
  - Per-post dashboards and channel rollups.
  - Export and scheduled reports.

9) Webhooks & Event Processing
- Best practices:
  - Verify signatures, respond quickly (200 OK), and hand off for processing.
  - De-duplicate by event id + tenant id.
  - Emit normalized events to Kafka topics for downstream consumers.

10) Security & Compliance
- Token storage: use Vault or KMS; rotate periodically.
- Access control: permissions for channel linking, publishing, and inbox access.
- Data retention: define retention policies for messages and media per-tenant.

11) Observability & Monitoring
- Key metrics: webhook ingestion rate, publish success rate, queue backlog, conversation SLA breaches.
- Trace events: include tenantId and channelId in traces for drill-down.

12) Developer Notes & SDK Hooks
- HostSDK extensions:
  - hostSdk.social.openComposer(initialDraft)
  - hostSdk.social.registerComposerExtension({ id, mount(container, hostSdk) })
  - hostSdk.social.onConversationUpdate(cb)

Appendix: Sample composer payload (POST /bff/social/posts)

```json
{
  "tenantId": "t-acme",
  "channels": ["instagram:insta-123","facebook:fb-456"],
  "type": "reel",
  "body": "Check our new Blue Jacket! #winter",
  "scheduledAt": "2026-03-01T10:00:00Z",
  "media": [{ "uploadId": "u-789", "type":"video", "rendition":"1080p" }],
  "productTags": [{ "sku":"BJ-001","x":0.5,"y":0.7 }],
  "meta": { "campaignId":"camp-12" }
}
```

