# QPub Documentation

Customer-facing documentation for QPub, written in MDX. Navigation is defined in `sidebar.json`.

## Directory structure

```
docs/
├── intro/              # Product overview
├── getting-started/    # Quickstart, SDKs, API overview
├── core/               # Auth, connection, channel, pub/sub, queue
├── api/                # Socket/REST SDK and protocol reference
├── dashboard/          # Dashboard product docs
├── pricing/            # Plans and limits
├── community/          # Contributing and support
├── changelog.mdx
└── sidebar.json
```

## Terminology

| Term                 | Meaning                                                          |
| -------------------- | ---------------------------------------------------------------- |
| Account              | Billing/org entity in the dashboard                              |
| Project              | Isolation boundary for messaging and queues                      |
| API key              | Credential `publicId:secret` (Basic auth), not username/password |
| Token / JWT          | Short-lived client credential issued from an API key             |
| Token request        | Signed payload exchanged for a JWT (safe for browsers)           |
| Alias                | Optional client identity string                                  |
| Channel              | Named pub/sub topic (string); no separate create step            |
| Event                | Optional name/filter on a message within a channel               |
| Connection           | WebSocket session                                                |
| Permission           | Map of resource keys → actions (`publish`, `subscribe`, …)       |
| Queue / Job / Worker | Durable async work via REST                                      |
| Socket client        | `QPub.Socket` (WebSocket)                                        |
| REST client          | `QPub.Rest` (HTTP)                                               |

## Hosts and package

| Service   | Default host              |
| --------- | ------------------------- |
| REST API  | `https://rest.qpub.io/v1` |
| WebSocket | `wss://socket.qpub.io/v1` |

JavaScript package: `@qpub/sdk` (React: `@qpub/sdk/react`).

Use `QPub.Socket` and `QPub.Rest` — not `QPubSocket` / `QPubREST`.

## Writing rules

1. Backend public routes and protocol are the source of truth for API behavior.
2. `@qpub/sdk` and `qpub-js-examples` are the source of truth for JavaScript examples.
3. Do not invent endpoints, SDKs, channel types, or auth methods.
4. Keep pages short and practical. Prefer runnable examples.
5. If something cannot be verified, mark it `TODO` instead of guessing.
6. Avoid marketing language and internal architecture (NATS, DDD, cluster details).
7. Internal cross-links must use the site prefix: `[Quickstart](/docs/getting-started/quickstart)`, not `/getting-started/...`.

## Contributing

1. Edit or add MDX under the directories above.
2. Update `sidebar.json` when adding or renaming pages.
3. Follow the terminology and hosts in this README.
