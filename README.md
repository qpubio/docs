# QPub Documentation

Customer-facing documentation for QPub, written in MDX. Navigation is defined in `sidebar.json`.

## Directory structure

```
docs/
├── intro/              # Product overview
├── getting-started/    # Quickstart, SDKs, API overview
├── core/               # Auth, connection, channel, pub/sub, queue
├── api/                # Socket/REST SDK and protocol reference
├── cloud/              # QPub Cloud product docs
├── pricing/            # Plans and limits
├── community/          # Contributing and support
├── changelog.mdx
└── sidebar.json
```

## Terminology

| Term                 | Meaning                                                            |
| -------------------- | ------------------------------------------------------------------ |
| Account              | Billing/org entity in QPub Cloud                                   |
| Project              | Isolation boundary for Channels and Queues                         |
| Channels (product)   | Real-time pub/sub product; primitive is a **channel** (topic name) |
| Queues (product)     | Durable background jobs product                                    |
| API key              | Credential `publicId:secret` (Basic auth), not username/password   |
| Token / JWT          | Short-lived client credential issued from an API key               |
| Token request        | Signed payload exchanged for a JWT (safe for browsers)             |
| Alias                | Optional client identity string                                    |
| Channel              | Named pub/sub topic (string); no separate create step              |
| Event                | Optional name/filter on a message within a channel                 |
| Connection           | WebSocket session                                                  |
| Permission           | Map of resource keys → actions (`publish`, `subscribe`, …)         |
| Queue / Job / Worker | Durable async work via REST                                        |
| Socket client        | `QPub.Socket` (WebSocket)                                          |
| REST client          | `QPub.Rest` (HTTP)                                                 |

## Hosts and package

| Service   | Default host              |
| --------- | ------------------------- |
| REST API  | `https://rest.qpub.io/v1` |
| WebSocket | `wss://socket.qpub.io/v1` |

Official SDKs: `@qpub/sdk` (JavaScript/TypeScript), `@qpub/sdk/react` (React), [qpub-go](https://pkg.go.dev/github.com/qpubio/qpub-go). Use `QPub.Socket` and `QPub.Rest` — not `QPubSocket` / `QPubREST`.

## Writing rules

1. Backend public routes and protocol are the source of truth for API behavior.
2. `@qpub/sdk` / `qpub-js-examples` and [qpub-go](https://github.com/qpubio/qpub-go) are sources of truth for SDK examples; public REST routes for cURL.
3. Do not invent endpoints, SDKs, channel types, or auth methods.
4. Keep pages short and practical. Prefer runnable examples.
5. If something cannot be verified, mark it `TODO` instead of guessing.
6. Avoid marketing language and internal architecture (NATS, DDD, cluster details).
7. Every page must include YAML frontmatter with `title` and `description` (used for page titles, meta tags, and search snippets).
8. Internal cross-links use repo-root paths with `.mdx` (GitHub file preview needs the extension): `[Quickstart](/getting-started/quickstart.mdx)`, indexes as `[Auth](/core/auth/index.mdx)`. The website rewrites these to `/docs/...` (strips `.mdx` / `/index`) — do not hardcode `/docs` in content.
9. MDX treats `{...}` as JavaScript. Do not use Pandoc-style heading IDs like `## Title {#id}`; rely on auto-generated slugs instead.
10. **Multi-language examples** — Prefer `<ExampleScope>` + `<CodeExamples>` for runnable SDK/REST samples. Props are parsed server-side for docs chrome SSR. **API** / **Language** controls live in docs chrome (right rail on `lg+`, above the sidebar on `md`–`lg`, docs drawer on mobile). In prose, point readers to those controls, not “Examples”.
    - **Chrome languages:** JavaScript, TypeScript, React, Go, and cURL where REST applies. Pick per page, e.g. `languages={["javascript","typescript","react","go","curl"]}` for Socket+REST tutorials; omit `curl` on Socket-only pages.
    - **Code:** `<CodeExamples group="Socket">` (or `REST`) with fenced `javascript`, `typescript`, `react`, `go`, and `curl` children. See `getting-started/quickstart.mdx`.
    - **Prose:** General copy uses product terms (Socket client, REST client, SDK) — not a specific language unless the section is lang-specific. Use `<When group lang>` for variant notes; `lang={["javascript","typescript"]}` when JS and TS share the same text. Hidden inactive branches stay in the DOM for SEO.
    - **Install:** `<InstallExamples>` with `npm` / `pnpm` / `yarn` / `bun` fences inside `<When lang={["javascript","typescript","react"]}>`. Go: `go get github.com/qpubio/qpub-go@v0.2.0` in `<When lang="go">`. Install tabs do not change the URL.
    - **SDK import table:** `<SdkImport href="…" lang="javascript|react|go|curl|…">label</SdkImport>` for the Import column (primary code pill + lang icon; `curl` uses the terminal icon). See `getting-started/sdks.mdx`.
    - **Go:** [qpub-go](https://pkg.go.dev/github.com/qpubio/qpub-go) v0.2.0+ (`import "github.com/qpubio/qpub-go"`).
    - **Skip ExampleScope** on pure cloud/pricing prose and static REST reference tables when no runnable snippet is needed.
11. The website loads MDX from this repo locally in development (`DOCS_CONTENT_PATH` or `../docs`). Production fetches GitHub `main` — push docs before expecting live site updates.

### New page checklist

- [ ] Frontmatter `title` + `description` (language-neutral where possible)
- [ ] Runnable samples in `<CodeExamples>` with all languages this page supports
- [ ] cURL on REST operations; REST SDK pages include `curl` fences
- [ ] Lang-specific notes in `<When>`, not in opening paragraphs
- [ ] Verify against backend routes + SDK repos before merging

## Contributing

1. Edit or add MDX under the directories above.
2. Update `sidebar.json` when adding or renaming pages.
3. Follow the terminology and hosts in this README.
