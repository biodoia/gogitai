# Gitea Package Registry + CI/CD Pipeline

> Nota: 2026-04-23 — Piano per binary distribution dell'ecosistema Biodoia.

## Responsabilità di GoGitAI

GoGitAI gestisce il backend Gitea. Quando Gitea è attivo, queste feature sono disponibili:

### 1. Package Registry (built-in Gitea)

Gitea 1.20+ include un Package Registry che supporta:
- **Generic packages** — binari arbitrari (quello che ci serve)
- **Go modules** — proxy privato per `go get`
- **Container** — Docker images
- **npm, PyPI, Maven, NuGet** — per altri linguaggi

Nessun setup extra — è una feature di Gitea abilitata di default.

### 2. Gitea Actions (CI/CD)

Compatibile con GitHub Actions. Richiede `act_runner` sulla macchina host.

Setup:
```bash
# Installa act_runner
wget https://gitea.com/gitea/act_runner/releases/latest/download/act_runner-linux-amd64
chmod +x act_runner-linux-amd64
./act_runner-linux-amd64 register --instance https://gitea.biodoia.ts.net --token TOKEN
./act_runner-linux-amd64 daemon
```

### 3. Pipeline per ogni repo FGT

Ogni repo dell'ecosistema avrà `.gitea/workflows/release.yml` che:
1. Triggera su tag `v*`
2. Cross-compila per linux/windows amd64/arm64
3. Carica binari su Gitea Package Registry
4. GoManageros (Layer 4) li scarica sulle macchine target

### 4. Go Module Proxy privato

Gitea funge anche da Go module proxy per i moduli privati:
```bash
export GOPROXY="https://gitea.biodoia.ts.net/api/packages/biodoia/go,direct"
export GOPRIVATE="gitea.biodoia.ts.net/*"
```

Questo elimina la necessità di `replace` directive nei go.mod.

## Dipendenze

- Gitea installato e configurato (responsabilità GoGitAI)
- act_runner attivo come systemd service
- Tailscale per accesso sicuro
- Token Gitea per auth (da goleciave)

## Stato: DA IMPLEMENTARE quando Gitea è attivo

Sequenza:
1. GoGitAI installa Gitea/Forgejo
2. GoGitAI configura act_runner
3. GoGitAI abilita Package Registry
4. Migrazione repo da GitHub a Gitea
5. GoManageros Layer 4 usa Package Registry per installare binari
