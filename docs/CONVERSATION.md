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

---

## Rivisitazione architetturale (22:48)

**Sergio:** "ipotizzando di installare forgejo, vorrei che lo wrappiamo in una app che si occupi di installarlo, aggiornarlo, gestire eventuali plugins, e questa app sarà appunto gogitai. così forgejo diventa semplicemente una dipendenza, non devo modificare nulla, ma avendo questo layer di management aggiuntivo, mi si aprono molte più opportunità. ad esempio, potrei volere 2 server, sia forgejo che soft serve, magari installando un connettore che li tenga sincronizzati a livello di repositories."

### Cambio di paradigma

Da: "gogitai = Forgejo wrapper"
A: **"gogitai = orchestratore multi-backend"**

Forgejo/Gitea/Soft Serve diventano **dipendenze**, non il prodotto.
GoGitAI è il layer di management che li installa, aggiorna, sincronizza.

### Rivalutazione Gitea

Sergio chiede: "siamo sicuri che gitea non sia meglio come ecosistema?"

Risposta: Gitea ha **20k+ Actions plugins**, VSCode plugin, JetBrains plugin, community più grande.
Forgejo ha governance più trasparente e test migliori.
Ma con gogitai come layer, **non bisogna scegliere** — si supportano entrambi come backend.

### Feature chiave emerse
- Lifecycle management: `gogitai install forgejo`, `gogitai update`, `gogitai rollback`
- Multi-backend sync: Forgejo ↔ Soft Serve mirror automatico
- Plugin system: sopra i backend, non dentro
- MCP server: agenti operano su repo autonomamente

### Embedded Browser UI

Sergio: "per l'interfaccia web di gogitai, possiamo avere varie dashboard, e poi dei tastoni che quando li clicco mi aprono l'interfaccia web di gitea o forgejo. oppure, se possibile, li possiamo fare embedded, dato che con le tecnologie di oggi si può avere un browser dentro ad un browser"

Soluzione: **dual mode**
1. **Embedded mode** — iframe sandboxed nella WebUI gogitai (l'utente non esce mai)
2. **External mode** — pulsante "apri in tab" per fullscreen

Dashboard multiple: overview, repo, CI/CD, agents, settings

### Security: Single Entry Point

Sergio: "così non mi devo preoccupare del reverse proxy di ciascuno, mi basta che la sicurezza dia sulla webpage di gogitai, senza dover esporre le webpage dei singoli"

- Backend girano su **localhost/internal ports** — mai esposti direttamente
- Solo gogitai ha TLS + auth + Tailscale
- Zero reverse proxy per backend individuali
- gogitai fa da gateway: auth → routing → iframe embed

### Visione completa (23:10)

Sergio ha delineato la visione completa di gogitai:

1. **Agent residente 24/7** — conversazione naturale per gestire tutto
2. **Plugin Store** — cards descrittive con toggle Android-style, agente che scandaglia il web
3. **Notification system** — aggiornamenti backend, plugin, build, PR, novità ghrego
4. **ghrego integration** — discovery di repo duplicati, nascosti, novità GitHub giornaliere, suggerimenti tipo "se uniamo questi 2 tuoi repo con questo 3° esterno → nuova app"
5. **govai integration** — deploy con 1 click dalla dashboard
6. **gociccidai integration** — CI/CD pipeline visuali, build logs real-time
7. **Bot Agent Pool** — agent h24 per auto PR, bug fixing, auto-merge, TODO list, code improvement, versioning
8. **Custom Git Interface** (Phase 2+) — interfaccia git personalizzata con solo funzioni utili + custom features

Ricerche condotte:
- **ComposioHQ/agent-orchestrator**: orchestrazione agenti paralleli in worktree isolati — ispirazione per Bot Agent Pool
- **Sourcebot**: code search self-hosted (alternativa Sourcegraph), Zoekt engine, MCP server — per ghrego integration
- **delightful-forgejo**: curated plugin list per Forgejo
- **Qodo Merge**: AI code review, auto PR descriptions — per PR review automatizzata

Documentazione formale in `docs/PLAN.md`

---

## Decisioni finali

1. ✅ **Forgejo** come backend default (Gitea supportato come alternativa)
2. ✅ **Soft Serve** come mirror SSH opzionale
3. ✅ **GoGitAI è l'orchestratore** — backend sono dipendenze swappabili
4. ✅ **Multi-backend sync** tra forge diversi
5. ✅ **FrameGoTUI** per dashboard (TUI + WebUI)
6. ✅ **MCP server** per operazioni git agentiche
7. ✅ **Tailscale** per accesso sicuro (zero porte pubbliche)
8. ✅ **Memogo** per storage (build, deploy, stato)
9. ✅ Progetto `gogitai` creato in `/home/lisergico25/projects/gogitai`

---

## Prossimi passi
- [ ] GoGitAI Core (Go skeleton con FrameGoTUI)
- [ ] Backend: Forgejo integration
- [ ] Backend: Soft Serve integration
- [ ] Sync connector
- [ ] MCP server
- [ ] Dashboard FrameGoTUI
- [ ] Plugin system
