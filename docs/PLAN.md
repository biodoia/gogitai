# 🦞 GoGitAI — Product Plan

## Visione

GoGitAI è il **centro nervoso git** dell'azienda agentica Autoschei.
Non è un git server — è l'orchestratore che fa lavorare insieme backend git, agenti AI,
e tutte le app dell'ecosistema (ghrego, govai, gociccidai) in un'unica interfaccia.

Ogni feature è accessibile via TUI (FrameGoTUI) + WebUI (HTMX) + chat con l'agente residente.

---

## 🔴 Core Modules

### 1. Agent Residente (gogitai-agent)
- **Daemon 24/7** come ogni app FrameGoTUI
- Chat naturale: "gogitai, come sta andando la build di framegotui?"
- Può fare tutto: installare backend, gestire plugin, creare repo, rispondere domande
- MCP server per interazione con altri agenti dell'ecosistema
- A2A per delega cross-app

### 2. Lifecycle Management (gogitai-core)
- Install backend: `gogitai install forgejo` / `gogitai install gitea` / `gogitai install soft-serve`
- Update con rollback automatico se qualcosa si rompe
- Health monitoring continuo
- Backup / Restore automatico via Memogo
- Config unificata per tutti i backend

### 3. Multi-Backend Sync
- Mirror bidirezionale tra backend (es. Forgejo ↔ Soft Serve)
- Sync connector configurabile (one-way, bi-directional, selective)
- Un push su Forgejo → automaticamente su Soft Serve via SSH
- Conflict detection e resolution assistita dall'agente

### 4. Security Gateway
- **Single entry point**: solo gogitai è esposto (via Tailscale)
- Backend girano su localhost/internal — mai accessibili direttamente
- TLS + auth gestiti una volta sola da gogitai
- Zero reverse proxy per backend individuali
- Session management unificato

---

## 🟡 WebUI Features

### 5. Dashboard Multiple
- **Overview**: stato backend, repo count, build status, agent activity
- **Repos**: lista repo con stats, ultimi commit, health
- **CI/CD**: pipeline in corso, storico, logs
- **Agents**: agent bot attivi, task assegnati, progress
- **Plugins**: catalogo, installati, aggiornamenti disponibili
- **Settings**: config backend, sync rules, notifications

### 6. Embedded Backend UI
- Le interfacce di Forgejo/Gitea/Soft Serve girano dentro iframe sandboxed
- L'utente non esce mai dalla dashboard gogitai
- Dual mode: embedded (default) o "apri in tab separata"
- Soft Serve embedded via xterm.js (terminal web)
- Comunicazione via postMessage tra gogitai e iframe

### 7. Plugin Store
- **Cards descrittive** con immagine, descrizione, rating, version
- **Tastino stile toggle Android**: click → scarica e installa
- L'agente scandaglia il web per nuovi plugin:
  - Gitea marketplace (20k+ Actions)
  - Forgejo ecosystem (delightful-forgejo)
  - Temi, integrations, hooks
- Notifiche push per aggiornamenti disponibili
- Auto-update configurabile per plugin trusted

### 8. Notification System
- Badge/counter su ogni dashboard
- Notifiche per:
  - Aggiornamenti backend disponibili
  - Aggiornamenti plugin
  - Build fallite/passate
  - PR in attesa di review
  - Agent bot che hanno completato task
  - Novità da ghrego (repo correlati, idee)
- Digest giornaliero configurabile via agente
- Priority: urgente → push, info → digest

---

## 🟢 Ecosystem Integrations

### 9. ghrego Integration (Repo Discovery)
ghrego è l'app che scandaglia tutti i repo e trova nessi. In gogitai:
- **Pagina "Discovery"**: "hei Sergio, guarda, è uscito il pezzo mancante per un'idea che avevamo"
- Cross-repo analysis: repo duplicati da unire, nascosti da esplorare
- Daily scan di novità GitHub: nuovi progetti rilevanti per il tuo stack
- Suggerimenti: "se uniamo questi 2 tuoi repo con questo 3° esterno → nuova app fighissima"
- Trending repos nel mondo Go, Charmbracelet, AI agents
- Metriche: star growth, fork, attività recente

### 10. govai Integration (1-Click Deploy)
- Pulsante "Deploy" su ogni repo nella dashboard
- govai gestisce il deployment reale
- gogitai mostra stato deploy, rollback, logs
- Deploy targets: Tailscale host, Docker, systemd service

### 11. gociccidai Integration (CI/CD)
- Pipeline visuali nella dashboard CI/CD
- Build logs in tempo reale
- Trigger manuali o automatici (push, PR, schedule)
- Artifacts e release management
- Matrix builds per multi-platform

### 12. Bot Agent Pool (Autonomous Workers)
Ispirato a [ComposioHQ/agent-orchestrator](https://github.com/ComposioHQ/agent-orchestrator):
- **Agent bot che lavorano h24** sulle repo:
  - Auto-fix di bug segnalati
  - Auto-merge di PR approvate
  - Lavoro sulla TODO list del codice
  - Miglioramento codice continuo (refactoring, docs, tests)
  - Versioning automatico e changelog
  - PR review automatizzata con AI
- Ogni bot lavora in **git worktree isolato** (branch separato)
- Dashboard "Agents" mostra: chi sta lavorando, su cosa, progress
- L'utente supervisiona, non micromanage
- Notifica solo quando serve decisione umana

### 12. Environment Tiers (dev / staging / prod)
- **3 ambienti isolati** su subnet Tailscale separate:
  - `dev` → sviluppo, rollback facile, dati fake, agenti sperimentano
  - `staging` → pre-produzione, dati reali mirrorati, test completi
  - `prod` → produzione, deploy solo dopo approval, monitored 24/7
- Ogni tier ha le sue istanze backend (Forgejo/Gitea), i suoi agenti, le sue config
- Promozione: dev → staging → prod con approval gate
- govai gestisce il deploy cross-tier
- gociccidai ha pipeline separate per tier
- Tailscale ACL per controllare chi accede a cosa
- Dashboard mostra stato di tutti e 3 i tier in un'unica vista

### 13. Spegoplain Integration (Spec-Driven Development)
- Spegoplain è l'agente SDD dell'ecosistema: scrive specs, pianifica sviluppo
- In gogitai: pagina "Roadmap" con specs e piani generati da Spegoplain
- Link specs → repo → branch → PR → CI/CD: flusso completo tracciabile
- L'agente residente: "gogitai, cosa manca per completare la spec X?" → risposta incrociando Spegoplain + stato repo + Progotti
- Auto-generazione di branch/PR dalla spec: Spegoplain pianifica → gogitai crea branch → Bot Agent Pool implementa
- Progress tracking: % spec completata basata su commit/PR/issue collegate

### 14. Progotti Integration (Project Management)
- Progotti è il gestore di progetti dell'ecosistema Autoschei
- In gogitai: pagina "Projects" con overview progetti, stato, milestone
- Link bidirezionale repo ↔ progetto: ogni repo collegata al suo progetto Progotti
- TODO list del progetto visibile nella dashboard repo
- L'agente sincronizza: chiude issue → aggiorna Progotti, completa milestone → tagga repo
- Board Kanban-style nella WebUI per visualizzare stato progetti
- L'agente residente: "gogitai, a che punto è il progetto framegotui?" → risposta da Progotti + stato repo + CI/CD

### 14. News Bollettino (Daily Hot Repos)
- Pagina "News" nella dashboard con feed giornaliero di repo hot
- Fonti aggregate:
  - **GitHub Trending** (scraping API)
  - **Trendshift.io** — alternativa open source a GitHub Trending, scoring algoritmico, trending per language/topic, supporta MCP
  - **daily.dev** — aggregatore developer news comunitario
  - **zread** — nostra skill di lettura/riassunto
  - **Hacker News** (API ufficiale)
  - **GitNews** (git.news) — trending da GitHub + HN + Reddit
  - **DevURLs** (devurls.com) — aggregatore developer news
  - **Hackertab** — browser extension con GitHub Trending + HN + DevTo + Product Hunt
- L'agente filtra per interessi (Go, Charmbracelet, AI agents, self-hosted)
- Notifica giornaliera: "Ehi Sergio, oggi in trending: [repo1], [repo2], [repo3]"
- Link diretto per mirror/sync con 1 click

### 15. Repo Sync (Stars, Watched, Dependencies, Private)
- Mirror automatico di:
  - Repo GitHub starred → Forgejo (auto-discovery)
  - Repo private vecchie → Forgejo (backup/sync)
  - Repo watched → mirror read-only
  - Dependencies (go.mod/go.sum) → mirror dipendenze Go
- Tool di riferimento:
  - **Gitea Mirror** (giteamirror.com) — WebUI, auto-discovery, scheduler, Helm chart
  - **GitHub-Backup** (clockfort) — backup repo user/org
  - **SierraSoftworks/github-backup** — backup stars, repos, gists, releases
  - **go-git/v5** — operazioni programmatiche per sync custom
- Dashboard "Sync" mostra stato mirror, ultimo sync, conflitti
- Per le dipendenze: parse go.mod → mirror su Forgejo
  - Se dependency scompare da upstream → hai il backup
  - Se esce aggiornamento → notifica + auto-mirror

### 16. Git Integration (Standard)
- Clone, push, pull, fetch via gogitai
- Branch management
- Tag e release
- Diff viewer nella WebUI
- Blame, history, annotate
- Tutto accessibile via TUI, WebUI, o chat agente

---

## 🔵 Future (Phase 2+)

### 14. Custom Git Interface
- Interfaccia git personalizzata, disegnata da Sergio
- Solo le funzionalità che servono, + custom features
- Potrebbe includere workflow git semplificati
- Visual git graph interattivo
- Smart commit messages generati dall'AI
- "Ma ci penseremo più avanti" — Sergio, 2026-04-22

---

## 🔍 Ricerche e Proposte

### Trovato: Agent Orchestrator (ComposioHQ)
- **Repo:** github.com/ComposioHQ/agent-orchestrator
- **Cosa fa:** orchestrazione di agenti AI in parallelo su codebase
- **Come:** ogni agente in worktree isolato, fix CI, risponde a review, apre PR
- **Per gogitai:** ispirazione diretta per il Bot Agent Pool (#12)
- **Da studiare:** architettura worktree isolation, CI feedback loop

### Trovato: Sourcebot
- **Repo:** github.com/sourcebot-dev/sourcebot
- **Cosa fa:** code search self-hosted cross-repo (alternativa Sourcegraph)
- **Stack:** Zoekt search engine, Docker, supporta GitHub/GitLab/Gitea/Bitbucket
- **Features:** regex, filtri, boolean, AI问答 con citations, 73ms search avg
- **Per gogitai:** integrare come search backend per la feature ghrego (#9)
- **Da studiare:** MCP server di Sourcebot per integrazione agentica

### Trovato: delightful-forgejo
- **Repo:** codeberg.org/forgejo-contrib/delightful-forgejo
- **Cosa fa:** curated list di plugin/tools Forgejo
- **Per gogitai:** fonte per il Plugin Store (#7)

### Trovato: Qodo Merge (ex PR-Agent)
- **Sito:** qodo.ai
- **Cosa fa:** AI code review auto-genera PR descriptions, catch bugs
- **Per gogitai:** integrare nel Bot Agent Pool per PR review automatizzata

---

## Architecture (Updated)

```
┌─────────────────────────────────────────────────────────────┐
│                         GoGitAI                              │
│                                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────────────┐ │
│  │ TUI Dashboard │  │ WebUI (HTMX) │  │ Agent Residente   │ │
│  │ (FrameGoTUI)  │  │              │  │ (chat 24/7)       │ │
│  └──────┬───────┘  └──────┬───────┘  └────────┬──────────┘ │
│         │                 │                    │             │
│  ┌──────┴─────────────────┴────────────────────┴─────────┐ │
│  │                   gogitai Core                         │ │
│  │                                                        │ │
│  │  ┌─────────────┐  ┌──────────────┐  ┌──────────────┐  │ │
│  │  │ Lifecycle   │  │ Plugin Store  │  │ Notifications│  │ │
│  │  │ Management  │  │ (auto-scan)   │  │ System       │  │ │
│  │  └─────────────┘  └──────────────┘  └──────────────┘  │ │
│  │                                                        │ │
│  │  ┌─────────────┐  ┌──────────────┐  ┌──────────────┐  │ │
│  │  │ Multi-Backend│  │ Security     │  │ Bot Agent    │  │ │
│  │  │ Sync        │  │ Gateway      │  │ Pool         │  │ │
│  │  └──────┬──────┘  └──────────────┘  └──────┬───────┘  │ │
│  └─────────┼───────────────────────────────────┼──────────┘ │
│            │                                    │            │
│  ┌─────────┼────────────────────────────────────┼──────────┐ │
│  │  Backends │          │            │           │          │ │
│  │  ┌────┴───┐  ┌───────┴──┐  ┌─────┴────┐     │          │ │
│  │  │Forgejo │  │ Gitea    │  │Soft Serve│     │          │ │
│  │  └────────┘  └──────────┘  └──────────┘     │          │ │
│  └──────────────────────────────────────────────┼──────────┘ │
│                                                  │            │
│  ┌──────────────────────────────────────────────┴──────────┐ │
│  │              Ecosystem Integrations                      │ │
│  │                                                          │ │
│  │  ┌──────────┐  ┌────────┐  ┌────────────┐  ┌────────┐  │ │
│  │  │ ghrego   │  │ govai  │  │ gociccidai  │  │ Memogo │  │ │
│  │  │ (discovery)│ (deploy)│  │ (CI/CD)     │  │ (data) │  │ │
│  │  └──────────┘  └────────┘  └────────────┘  └────────┘  │ │
│  └──────────────────────────────────────────────────────────┘ │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │              Tailscale (mesh network)                     │ │
│  └──────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────┘
```

---

## Development Phases

### Phase 1: Foundation ⬅️ INIZIO
- [ ] Go skeleton con FrameGoTUI (1 backend + 2 frontend)
- [ ] gogitai-agent daemon 24/7 con chat base
- [ ] Forgejo integration (install, config, health)
- [ ] Security gateway (Tailscale + embedded iframe)
- [ ] Memogo integration per stato

### Phase 2: Management UI
- [ ] Dashboard multiple (overview, repos, CI/CD, agents, settings)
- [ ] Embedded backend UI (iframe + xterm.js)
- [ ] Plugin Store (cards + toggle install)
- [ ] Notification system

### Phase 3: Ecosystem
- [ ] ghrego integration (discovery page)
- [ ] govai integration (1-click deploy)
- [ ] gociccidai integration (CI/CD pipeline)
- [ ] Multi-backend sync connector

### Phase 3.5: News & Sync
- [ ] News Bollettino (daily trending feed)
- [ ] Dependency/Star/Private repo sync

### Phase 4: Autonomous Agents
- [ ] Bot Agent Pool (worktree isolation)
- [ ] Auto-fix, auto-merge, PR review
- [ ] TODO list processing
- [ ] Versioning automation
- [ ] Code improvement h24

### Phase 5: Custom Git (future)
- [ ] Interfaccia git personalizzata
- [ ] Smart commit messages
- [ ] Visual git graph
- [ ] Workflow semplificati
