# Conversazione Fondativa — GoGitAI
## 2026-04-22, 19:07–22:39 Europe/Rome

### Partecipanti
- **Sergio Martinelli** (@Biodoia)
- **Cicciobot** (AI assistant)

---

## Richiesta iniziale (19:07)

**Sergio:** "usando la skill di ricerca biblaigo fammi una ricerca approfondita per trovare il migliore server git self hosted basato su go"

### Ricerca condotta

Tentato biblaigo (daemon non attivo), poi ricerca web approfondita con query:
- "best self-hosted git server written in Go Golang 2024 2025"
- "Gitea vs Gogs vs Forgejo comparison self-hosted git 2025"
- "soft-serve git server Go charmbracelet self-hosted"

### Candidati valutati

#### 1. Gitea (~45k⭐)
- **Stack:** Go + SQLite/PostgreSQL/MySQL
- **Licenza:** MIT
- **Features:** Web UI completa, CI/CD (Gitea Actions compatibili GitHub Actions), package registry, code review, wiki, issues, pull requests
- **Pro:** Feature-rich, community attivissima, aggiornamenti frequenti
- **Contro:** Progetto passato sotto Gitea Ltd. — governance commerciale

#### 2. Forgejo (~8k⭐)
- **Stack:** Go + SQLite/PostgreSQL/MySQL
- **Licenza:** MIT → GPLv3+ (dal 2024)
- **Features:** Stesse di Gitea + Forgejo Actions, governance democratica, storage unificato, upgrade tests automatizzati
- **Pro:** FOSS al 100%, governance comunitaria trasparente, migrazione da Gitea seamless, test di qualità superiori
- **Contro:** Community più piccola di Gitea

#### 3. Gogs (~45k⭐)
- **Stack:** Go + SQLite/PostgreSQL/MySQL
- **Licenza:** MIT
- **Features:** Web UI, issues, pull requests, wiki
- **Pro:** Singolo binario, pochissima RAM (~30MB), perfetto per VPS piccoli
- **Contro:** Sviluppo lentissimo, mantenuto da una sola persona, niente CI/CD nativo

#### 4. Soft Serve (~5k⭐, Charmbracelet)
- **Stack:** Go
- **Licenza:** MIT
- **Features:** TUI via SSH, gestione repo via comandi SSH, syntax highlighting
- **Pro:** Perfetto per terminal lover, single binary, Charmbracelet style
- **Contro:** Niente web UI, niente issues/PR/wiki, non per team

---

## Contesto strategico (20:14)

**Sergio:** "datti un occhiata al mio sdk framegotui. io sto puntando ad una azienda agentica ai indipendete al 100%, inquadrilo in questo contesto"

### Analisi di FrameGoTUI nell'ecosistema

Il framework è stato inquadrato come **layer di presentazione** dell'architettura agentica Autoschei:

- **25+ moduli Go** in `/home/lisergico25/repos/`
- **3 layer:** Orchestration (GoBro, GOLEM, Progotti) → Execution (gociccidai, goaiaiai, ghrego, govai) → Memory (memogo, cipher)
- **Architettura 1 Backend + 2 Frontend:** ogni app ha un daemon gRPC + TUI (Charmbracelet) + WebUI (HTMX)
- **Protocolli agentici:** MCP (Anthropic), A2A (Google), Agent Protocol
- **Memogo** come sistema nervoso centralizzato — nessun DB autonomo
- **Tailscale** per mesh networking sicura

### Perché la scelta ricade su Forgejo

Per un'azienda agentica 100% indipendente:
1. **Self-hosted** → nessun GitHub/GitLab esterno
2. **Go** → coerenza con tutto lo stack
3. **Integrabile con MCP/A2A** → agenti operano su repo autonomamente
4. **CI/CD integrato** → Forgejo Actions = GitHub Actions compatible
5. **FOSS puro** → governance trasparente, nessun rischio commerciale

---

## Decisioni finali

1. ✅ **Forgejo** come git forge
2. ✅ **FrameGoTUI** per dashboard (TUI + WebUI)
3. ✅ **MCP server** per operazioni git agentiche
4. ✅ **Tailscale** per accesso sicuro
5. ✅ **Memogo** per storage (build, deploy, stato)
6. ✅ Progetto `gogitai` creato in `/home/lisergico25/projects/gogitai`

---

## Prossimi passi
- [ ] Installare Forgejo su x870 via Tailscale
- [ ] Creare MCP server per Forgejo
- [ ] Dashboard FrameGoTUI
- [ ] Integrazione agenti Autoschei
