# 🦞 GoGitAI

**Git server management layer AI-native** — Orchestratore multi-backend per git self-hosted

GoGitAI non è un git server. È il **layer di management** che wrappa, installa, aggiorna e sincronizza
qualsiasi backend git self-hosted (Forgejo, Gitea, Soft Serve). Tutto integrato nell'ecosistema Autoschei.

## Filosofia

> **I backend git sono dipendenze, non il prodotto.**
>
> GoGitAI è l'orchestratore. Forgejo/Gitea/Soft Serve sono swappabili.
> Questo apre porte: multi-backend, sync, plugin management, monitoring,
> tutto sotto un'unica interfaccia FrameGoTUI.

## Stack

- **GoGitAI Core** — Orchestratore Go (install, update, rollback, sync, plugins)
- **Backend supportati:** Forgejo, Gitea, Soft Serve (swappabili)
- **FrameGoTUI** — Dashboard TUI cyberpunk + WebUI HTMX
- **MCP Server** — Protocollo agentico per operazioni git
- **A2A** — Agent-to-Agent per delega task tra agenti
- **Memogo** — Storage centralizzato (stato repo, build, deploy)
- **Tailscale** — Accesso sicuro via mesh, zero porte aperte

## Backends valutati (2026-04-22)

| Progetto | Scritto in | Licenza | CI/CD | Web UI | Ecosistema | Note |
|----------|-----------|---------|-------|--------|------------|------|
| **Gitea** | Go | MIT | ✅ Actions | ✅ | ✅ 20k+ plugins | Community grande, governance commerciale |
| **Forgejo** | Go | GPLv3+ | ✅ Actions | ✅ | ⚠️ Più piccolo | FOSS puro, governance democratica |
| **Gogs** | Go | MIT | ❌ | ✅ | ❌ | Lightweight, sviluppo quasi fermo |
| **Soft Serve** | Go | MIT | ❌ | ❌ | ❌ | TUI via SSH (Charmbracelet), no forge |

**Approccio: supportare multi-backend.** Forgejo come default per FOSS, Gitea per ecosistema più ampio, Soft Serve come mirror SSH-only.

## Architettura

```
┌─────────────────────────────────────────────────────────┐
│                      GoGitAI                             │
│             (Management Layer in Go)                     │
│                                                          │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │ TUI Dashboard│  │ WebUI (HTMX) │  │  MCP Server   │  │
│  │ (FrameGoTUI)│  │              │  │  (per agenti)  │  │
│  └──────┬──────┘  └──────┬───────┘  └───────┬───────┘  │
│         │                │                   │           │
│  ┌──────┴────────────────┴───────────────────┴───────┐  │
│  │              gogitai Core (Go)                      │  │
│  │  • Install / Update / Rollback di backend          │  │
│  │  • Plugin management                               │  │
│  │  • Multi-backend sync (Forgejo ↔ Soft Serve)       │  │
│  │  • Repo mirroring / sync connector                 │  │
│  │  • Config unificata                                │  │
│  │  • Health monitoring                               │  │
│  │  • Backup / Restore                                │  │
│  └──────┬──────────────┬──────────────┬─────────────┘  │
│         │              │              │                  │
│  ┌──────┴──────┐ ┌─────┴──────┐ ┌────┴───────────┐    │
│  │  Backend:   │ │ Backend:   │ │ Backend:       │    │
│  │  Forgejo    │ │  Gitea     │ │  Soft Serve    │    │
│  │  (default)  │ │            │ │  (mirror SSH)  │    │
│  └─────────────┘ └────────────┘ └────────────────┘    │
│         │              │              │                  │
│  ┌──────┴──────────────┴──────────────┴──────────────┐ │
│  │              Memogo (storage/memoria)               │ │
│  └────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────┐ │
│  │              Tailscale (mesh network)               │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

## Cosa può fare gogitai

### Lifecycle Management
- Installare backend git con un comando (`gogitai install forgejo`)
- Aggiornare senza downtime (`gogitai update`)
- Rollback se qualcosa si rompe
- Gestire config da un unico posto

### Multi-Backend Sync
- Mirror repo tra Forgejo e Soft Serve automaticamente
- Un repo pushato su Forgejo → appare su Soft Serve via SSH
- Connettore configurabile per tipo di sync (one-way, bi-directional)

### Plugin System
- Forgejo/Gitea Actions sono già plugin
- GoGitAI aggiunge un layer di plugin propri (monitoring, backup, notifiche)

### AI Integration
- MCP server: gli agenti creano repo, fanno PR, gestiscono CI/CD
- A2A: delega task git ad altri agenti dell'ecosistema
- Dashboard FrameGoTUI: tutto visibile da TUI e WebUI

## Decisioni della conversazione fondativa (2026-04-22)

Vedi `docs/CONVERSATION.md` per il resoconto completo.

### Riassunto decisioni:
1. ✅ **Forgejo** come backend default (Gitea supportato come alternativa)
2. ✅ **Soft Serve** come mirror SSH opzionale
3. ✅ **GoGitAI** è l'orchestratore, non il server — backend sono dipendenze
4. ✅ **Multi-backend sync** tra forge diversi
5. ✅ **FrameGoTUI** per la dashboard (TUI + WebUI)
6. ✅ **MCP server** per operazioni git agentiche
7. ✅ **Tailscale** per esposizione sicura (zero porte pubbliche)
8. ✅ **Memogo** per storage (build, deploy, stato repo)

## Stato

- [x] Ricerca server git Go completata
- [x] Architettura multi-backend definita
- [x] Progetto inizializzato
- [ ] GoGitAI Core (Go skeleton con FrameGoTUI)
- [ ] Backend: Forgejo integration
- [ ] Backend: Soft Serve integration
- [ ] Sync connector
- [ ] MCP server
- [ ] Dashboard FrameGoTUI
- [ ] Plugin system
