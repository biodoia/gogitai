# 🦞 GoGitAI

**Self-hosted Git server AI-native** — Forgejo + MCP + FrameGoTUI

Un server git self-hosted scritto in Go, completamente integrato nell'ecosistema Autoschei.
Gli agenti gestiscono repo, PR, CI/CD in completa autonomia.

## Stack

- **Forgejo** — Git forge FOSS (fork di Gitea), Go, MIT→GPLv3+
- **FrameGoTUI** — Dashboard TUI cyberpunk + WebUI HTMX
- **MCP Server** — Protocollo agentico per operazioni git
- **A2A** — Agent-to-Agent per delega task tra agenti
- **Memogo** — Storage centralizzato (stato repo, build, deploy)
- **Tailscale** — Accesso sicuro via mesh, zero porte aperte

## Perché Forgejo

Dalla ricerca approfondita del 2026-04-22, i candidati erano:

| Progetto | Scritto in | Licenza | CI/CD | Web UI | Note |
|----------|-----------|---------|-------|--------|------|
| **Gitea** | Go | MIT | ✅ Actions | ✅ | Community grande, ma governance commerciale |
| **Forgejo** | Go | GPLv3+ | ✅ Actions | ✅ | **FOSS puro, governance democratica** |
| **Gogs** | Go | MIT | ❌ | ✅ | Lightweight ma sviluppo quasi fermo |
| **Soft Serve** | Go | MIT | ❌ | ❌ | TUI bellissimo (Charmbracelet), ma no forge features |

**Scelta: Forgejo** — FOSS al 100%, CI/CD compatibile GitHub Actions, API completa per integrazione agentica, community attiva e trasparente.

## Architettura

```
┌──────────────────────────────────────────────────┐
│                  GoGitAI                          │
│                                                   │
│  ┌─────────────────────────────────────────────┐ │
│  │         Forgejo (git forge core)            │ │
│  │  • Repository management                    │ │
│  │  • Forgejo Actions (CI/CD)                  │ │
│  │  • API REST + webhooks                      │ │
│  │  • Issues, PR, code review                  │ │
│  └──────────────────┬──────────────────────────┘ │
│                     │                             │
│         ┌───────────┼───────────┐                 │
│         ▼           ▼           ▼                 │
│  ┌────────────┐ ┌─────────┐ ┌──────────┐        │
│  │ MCP Server │ │ A2A     │ │ FrameGo  │        │
│  │ (tools per │ │ (agent  │ │ TUI dash │        │
│  │  agent AI) │ │  delega)│ │ + WebUI  │        │
│  └──────┬─────┘ └────┬────┘ └────┬─────┘        │
│         │            │           │                │
│  ┌──────┴────────────┴───────────┴──────┐        │
│  │           Memogo (storage)            │        │
│  │  Build status, deploy state, metrics  │        │
│  └──────────────────────────────────────┘        │
│                     │                             │
│  ┌──────────────────┴──────────────────────┐     │
│  │        Tailscale (mesh network)         │     │
│  │   Accesso solo da rete privata 100.x    │     │
│  └─────────────────────────────────────────┘     │
└──────────────────────────────────────────────────┘
```

## Decisioni della conversazione fondativa (2026-04-22)

Vedi `docs/CONVERSATION.md` per il resoconto completo.

### Riassunto decisioni:
1. ✅ **Forgejo** come git forge (non Gitea, non Gogs, non Soft Serve)
2. ✅ **FrameGoTUI** per la dashboard (TUI + WebUI)
3. ✅ **MCP server** per operazioni git agentiche
4. ✅ **Tailscale** per esposizione sicura (zero porte pubbliche)
5. ✅ **Memogo** per storage (build, deploy, stato repo)

## Stato

- [x] Ricerca server git Go completata
- [x] Forgejo scelto
- [x] Progetto inizializzato
- [ ] Forgejo installato su x870
- [ ] MCP server per Forgejo
- [ ] Dashboard FrameGoTUI
- [ ] Integrazione agenti Autoschei
