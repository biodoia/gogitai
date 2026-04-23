# 🔍 GoGitAI — Ricerca Approfondita (2026-04-23)

Ricerca condotta su 8 aree chiave per il progetto GoGitAI.
Ogni sezione include strumenti, librerie e tecnologie rilevanti.

---

## 1. Git Forge Backend (Forgejo / Gitea / Soft Serve)

### Verdetto 2026: Forgejo è il default per self-hosting
- **DanubeData (11h fa):** "Per il 90% dei nuovi self-hosters nel 2026, Forgejo è il default più sicuro"
- **PkgPulse:** "Gitea per feature-rich, Forgejo per community-governed con federation, Gogs per semplicità"
- **Forgejo v1.2.0 (2026):** supporto plugin migliorato, repository management potenziato
- **Forgejo è ora un hard fork** da Gitea — versioni non più compatibili
- **Gitea è Open Core**: la versione hosted ha codice non disponibile nella versione free

### Per gogitai:
- Supportare **entrambi** come backend (API quasi identica)
- Forgejo come default, Gitea come alternativa per chi vuole enterprise features
- Soft Serve come mirror SSH-only (Charmbracelet, gestito via SSH API)

---

## 2. Git Operations in Go (librerie)

### ⭐ go-git/go-git (v5)
- **Repo:** github.com/go-git/go-git
- **Cos'è:** Implementazione Git **pura Go** — zero dipendenze esterne, no CGO
- **Features:** Clone, Push, Pull, Fetch, Checkout, Branch, Tag, Log, Diff, Worktree
- **Per gogitai:** È la libreria fondamentale per:
  - Multi-backend sync (clone da Forgejo → push a Soft Serve)
  - Custom Git Interface (Phase 5)
  - Bot Agent Pool (operazioni git programmatiche)
  - Repo mirroring
- **Usata da:** Sourcebot, Backstage, e tanti altri progetti Go
- **Install:** `go get github.com/go-git/go-git/v5`

### Perché go-git è perfetto per gogitai:
```
gogitai core → go-git → operazioni git pure Go
                        ├── clone/push per sync
                        ├── worktree per bot agents
                        ├── branch/tag per lifecycle
                        └── diff/log per dashboard
```

---

## 3. Git Repository Sync (Multi-Backend Mirror)

### ⭐ git-mirror (RalfJung)
- **Repo:** github.com/RalfJung/git-mirror
- **Cos'è:** Tieni repo su server multipli in sync
- **Come:** Webhook-based — push avviene, server pull e sincronizza
- **Per gogitai:** Ispirazione per il sync connector multi-backend

### ⭐ Gitea Mirror
- **Sito:** giteamirror.com
- **Cos'è:** Web app per mirror automatico GitHub → Gitea
- **Features:** Helm chart per K8s, continuous sync, WebUI
- **Per gogitai:** UX reference per il pannello sync nella dashboard

### Forgejo/Gitea built-in push mirror
- **Entrambi** supportano push mirror nativo (Settings → Mirror)
- gogitai può configurarlo via API automaticamente

### Approccio raccomandato per gogitai:
1. go-git per le operazioni programmatiche
2. Webhook dai backend per trigger
3. Config via gogitai → automatic setup di mirror su Forgejo/Gitea

---

## 4. Autonomous AI Coding Agents (Bot Agent Pool)

### ⭐⭐ ComposioHQ/agent-orchestrator
- **Repo:** github.com/ComposioHQ/agent-orchestrator
- **Cos'è:** Orchestrazione agenti AI paralleli su codebase
- **Pattern chiave:**
  - Ogni agente lavora in **git worktree isolato**
  - Branch separato → PR separata
  - Auto-fix CI failures
  - Auto-address review comments
  - Dashboard di supervisione
- **Per gogitai:** ARCHITETTURA DIRETTA per il Bot Agent Pool
  - Copiare il pattern worktree isolation
  - Dashboard supervisione (come la loro)
  - CI feedback loop

### ⭐⭐ PR-Agent (CodiumAI/Qodo)
- **Repo:** github.com/CodiumAI/pr-agent (10.5k⭐, 1.3k fork)
- **Cos'è:** AI code review open source
- **Features:**
  - Auto-genera PR descriptions
  - AI code review con suggestions
  - Auto-fix suggestions
  - Supporta Claude, GPT, Gemini, locali via Ollama
- **v0.32 (Feb 2026):** Supporto Claude Opus 4.6, Sonnet 4.6, Gemini 3 Pro
- **Per gogitai:** Integrare come tool per PR review automatizzata

### ⭐ OpenHands (ex OpenDevin)
- **Sito:** openhands.dev
- **Cos'è:** Piattaforma agenti AI per sviluppo software
- **Pattern:** issue → agente → piano → esecuzione → PR con test
- **Per gogitai:** Flusso "issue → bot agent → PR" da replicare

### ⭐ Cline
- **Cos'è:** Il "daily driver" coding agent nel 2026
- **Per gogitai:** IDE integration come riferimento per l'agent residente

### Altri tool review AI:
- **Greptile** — code graph, multi-hop investigation, v3 usa Claude Agent SDK
- **Graphite Agent** — non solo bot, compagno interattivo nelle PR

---

## 5. Code Search & Repository Intelligence (ghrego integration)

### ⭐⭐ Sourcebot
- **Repo:** github.com/sourcebot-dev/sourcebot
- **Cos'è:** Alternativa self-hosted a Sourcegraph
- **Stack:** Zoekt search engine, Docker
- **Features:**
  - Ricerca cross-repo (73ms media)
  - Regex, filtri, boolean, branch search
  - AI Q&A con citations (LLM cerca nel tuo codice)
  - Supporta GitHub, GitLab, Gitea, Bitbucket
  - **Ha MCP server!**
- **Per gogitai:** PERFETTO per ghrego integration
  - Embed Sourcebot come search backend
  - MCP server di Sourcebot → agenti possono cercare codice
  - Cross-repo analysis per trovare duplicati e nessi

### emerge (glato/emerge)
- Visualizzazione codebase e dependency graph
- Graph metrics, code quality, louvain modularity
- Export D3, GraphML, JSON
- Per gogitai: generazione grafi di dipendenze per ghrego

### gitinspector
- Statistical analysis per git repos
- Per gogitai: metriche contributi, attività

---

## 6. CI/CD (Forgejo Actions + gociccidai)

### Forgejo Actions
- **Compatibile con GitHub Actions** (stessa sintassi YAML)
- Variabili `FORGEJO_*` e `GITHUB_*` equivalenti
- Runner self-hosted (binary separato)
- Workflow in `.forgejo/workflows/`
- Docker-based job execution
- **DEFAULT_ACTIONS_URL** in app.ini per risolvere Actions da GitHub
- Runner v7.0.0+ per nuove features

### Per gogitai:
- gociccidai wrappa Forgejo Actions
- gogitai gestisce lifecycle del runner (install, update)
- Dashboard mostra pipeline, logs, artifacts

---

## 7. Authentication & SSO (Security Gateway)

### OAuth2/OIDC Support
- **Forgejo e Gitea** supportano entrambi:
  - OAuth2 Provider (per far autenticare altre app)
  - OAuth2/OIDC Client (per autenticarsi via provider esterno)
- **Authelia** → SSO self-hosted, integra con Forgejo via OIDC
- **Authentik** → SSO self-hosted, integration guide con Forgejo
- **Pocket ID** → lightweight OIDC per Forgejo

### Per gogitai:
- gogitai come **SSO gateway**:
  - Auth unica → accesso a tutti i backend
  - gogitai è l'OAuth2 provider
  - Backend sono OAuth2 clients di gogitai
  - Tailscale fornisce il layer network

---

## 8. Environment Tiers (dev/staging/prod)

### GitOps Approaches
- **Branch-per-environment:** `main` (prod), `staging` (staging), `develop` (dev)
- **Folder-per-environment:** `envs/dev/`, `envs/staging/`, `envs/prod/`
- **Repo-per-environment:** repo separati per ambiente

### Promotion Flow
```
dev → commit → auto-deploy su subnet dev
    → test passati → promote button → merge to staging
    → staging test → approval gate → promote to prod
    → tag release → deploy su subnet prod
```

### Tailscale per isolamento:
- **Tailscale ACLs** per controllare accesso per subnet
- Ogni tier su subnet dedicata
- Certificati HTTPS automatici per ogni tier

---

## 9. Embedded Terminal (Soft Serve Web UI)

### ⭐ xterm.js
- **Repo:** github.com/xtermjs/xterm.js
- **Cos'è:** Terminal emulator per il web (TypeScript)
- **Usato da:** VS Code, Codecademy, e tanti altri
- **Features:**
  - Full terminal emulation
  - WebSocket-based
  - Themes, addons (fit, search, webgl)
- **Per gogitai:** Embeddare Soft Serve nella WebUI
  - xterm.js + WebSocket proxy → SSH a Soft Serve
  - L'utente naviga le repo Soft Serve direttamente nel browser

### WeTTY
- **Repo:** github.com/butlerx/wetty
- Terminal in browser via xterm.js + WebSockets
- Alternativa più semplice per SSH web-based

---

## 10. Go SDK per Backend API

### ⭐ Gitea Go SDK
- **Repo:** gitea.com/gitea/go-sdk
- **Import:** `code.gitea.io/sdk/gitea`
- **Cos'è:** Client Go completo per API Gitea/Forgejo
- **Features:** Repos, Issues, PR, Users, Orgs, Settings, Actions, Packages
- **Per gogitai:** Libreria fondamentale per parlare con i backend
  - Lifecycle management (install, config)
  - Plugin discovery e install
  - Webhook setup
  - User/org management
  - Actions runner management

### ⭐ Forgejo MCP Server
- **Repo:** github.com/kunde21/forgejo-mcp
- **Cos'è:** MCP server per Forgejo/Gitea
- **Usa:** MCP SDK Go ufficiale (v0.4.0)
- **Per gogitai:** Già pronto! Da integrare nel MCP layer di gogitai

### Soft Serve Go API
- Gestito via **SSH API** (non REST)
- Comandi SSH: `repo create`, `repo import`, `user add`, etc.
- gogitai può wrappare questi comandi via Go SSH client

---

## 📊 Summary: Tools & Libraries per GoGitAI

| Area | Tool/Lib | Tipo | Importanza |
|------|----------|------|------------|
| Git ops Go | go-git/go-git v5 | Libreria | 🔴 Fondamentale |
| Forgejo/Gitea API | code.gitea.io/sdk/gitea | SDK | 🔴 Fondamentale |
| Forgejo MCP | kunde21/forgejo-mcp | MCP Server | 🔴 Fondamentale |
| Agent orchestration | ComposioHQ/agent-orchestrator | Reference | 🔴 Architettura |
| PR AI review | CodiumAI/pr-agent | Tool | 🟡 Integrazione |
| Code search | sourcebot-dev/sourcebot | Tool + MCP | 🟡 ghrego |
| Terminal web | xterm.js | Libreria JS | 🟡 Soft Serve embed |
| Repo sync | RalfJung/git-mirror | Reference | 🟢 Ispirazione |
| Code graph | glato/emerge | Tool | 🟢 ghrego vis |
| SSO | Authelia/Authentik | Service | 🟢 Security |

---

## 🎯 Prossimi passi suggeriti (dalla ricerca)

1. **Prima cosa:** studiare `go-git/v5` e `code.gitea.io/sdk/gitea` — sono le fondamenta
2. **Studiare** `agent-orchestrator` per il pattern worktree isolation
3. **Testare** `forgejo-mcp` esistente — potrebbe fare già molto del lavoro MCP
4. **Valutare** Sourcebot per la search — ha già MCP server integrato
5. **xterm.js** per l'embed di Soft Serve — standard de facto

---

## 11. News & Trending Sources (Bollettino)

### ⭐⭐ Trendshift.io
- **Sito:** trendshift.io
- **Cos'è:** Alternativa open source a GitHub Trending
- **Features:** scoring algoritmico (non solo star), trending per language/topic, monthly insights, MCP support
- **Per gogitai:** fonte primaria per il bollettino news + integrazione MCP

### daily.dev
- **Sito:** daily.dev
- **Cos'è:** Aggregatore developer news comunitario, feed personalizzabile
- **Per gogitai:** fonte secondaria per articoli e news

### GitNews
- **Sito:** git.news
- **Cos'è:** Trending repos da GitHub + Hacker News + Reddit in un'unica interfaccia
- **Per gogitai:** aggregatore multi-piattaforma

### DevURLs
- **Sito:** devurls.com
- **Cos'è:** Aggregatore developer news minimale
- **Per gogitai:** fonte leggera

### Hackertab.dev
- **Repo:** github.com/medyo/hackertab.dev
- **Cos'è:** Browser extension (Chrome + Firefox) per new tab con GitHub Trending + HN + DevTo + Product Hunt
- **Per gogitai:** UX reference per il bollettino, pattern di aggregazione

### Hacker News API
- **Repo:** github.com/HackerNews/API
- **API pubblica:** gratuito, no auth, Firebase-based
- **Per gogitai:** fonte per tech news generali

### Altre fonti:
- **GitHub Trending** (scraping, no API ufficiale)
- **Product Hunt** (API disponibile)
- **Lobsters** (comunità tech, RSS)
- **RSS generici** (FreshRSS/Miniflux per aggregazione self-hosted)

---

## 12. Repo Sync & Backup (Stars, Private, Dependencies)

### ⭐⭐ Gitea Mirror
- **Sito:** giteamirror.com
- **Cos'è:** Web app self-hosted per mirror GitHub → Gitea/Forgejo
- **Features:**
  - Auto-discovery di nuove repo GitHub
  - Backup automatico starred repos in org dedicata
  - Scheduler con cleanup automatico
  - Helm chart per K8s
  - Handle orphaned repositories (archive)
- **Per gogitai:** riferimento diretto per il modulo sync

### GitHub-Backup (clockfort)
- **Repo:** github.com/clockfort/GitHub-Backup
- **Cos'è:** Backup automatico tutte le repo di un user/org GitHub
- **Per gogitai:** script di riferimento per backup iniziale

### SierraSoftworks/github-backup
- **Repo:** github.com/SierraSoftworks/github-backup
- **Cos'è:** Backup strutturato con YAML config
- **Features:** backup stars, repos, gists, releases, filtrabile
- **Per gogitai:** config YAML come riferimento per il nostro sync config

### Sync Dependencies (go.mod/go.sum)
- **Approccio:** parse go.mod → lista dependencies → per ogni repo → git clone --mirror su Forgejo
- **go-git/v5** per le operazioni programmatiche
- **Benefit:** se una dependency scompare da GitHub, hai il mirror locale
- **gogitai aggiunge:** notifica aggiornamenti dependency + auto-sync

### Pattern per gogitai sync module:
```
Fonti:
  GitHub stars → auto-mirror Forgejo
  GitHub private repos → mirror + sync bidirezionale
  go.mod dependencies → mirror read-only
  Watched repos → mirror read-only

Trigger:
  Scheduler (ogni 6h default)
  Webhook (push GitHub → pull + sync)
  Manuale (bottone sync in dashboard)

Output:
  Mirror su Forgejo (org dedicata per tipo)
  Stato in Memogo
  Notifiche per nuove repo/aggiornamenti
```
