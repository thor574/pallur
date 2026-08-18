# pallur

Pallur er en beskrivelse av min KI-plattform for arbeid med agenter, lokale modeller,
utviklingsverktøy og kiitos-styring.

> Prosjektets KI- og arbeidsinstruks: [`.kiitos/prosjektinstruks.md`](.kiitos/prosjektinstruks.md).

Navnet er islandsk for plattform. Prosjektet starter som `v1 pallur`, men skal kunne
endres etter hvert som verktøy, modeller og arbeidsmåter modnes.

## Dokumentasjon

- [Lokal KI-plattform](docs/ki-plattform/README.md)
- [Agentarbeid og harness](docs/ki-plattform/agentarbeid.md)
- [Plattformskisse](docs/00-plattformskisse.md)
- [v0.1: Ollama og Continue](docs/10-v0.1-ollama-continue.md)
- [v1: LM Studio og Pi](docs/20-v1-lm-studio-pi.md)

## Status

- `v0.1` er dokumentert som en tidligere mulighet basert på eksisterende kiitos-ruff-notater.
- `v1` er testet med LM Studio + Qwen3.6-35B-A3B. Ytelsen er for lav for daglig bruk på 4 GB VRAM.
- **Lokal KI er parkert per 2026-06-30.** Se [praktiske funn](docs/ki-plattform/README.md) og [beslutningslogg](docs/ki-plattform/beslutningslogg.md) for detaljer og betingelser for gjenopptak.
- **Agentharness-sporet er åpent for små, kontrollerte forsøk per 2026-08-18.** Første aktuelle mønster er to rimelige agenter med separate Git-worktrees, tydelige roller og menneskelig diff-godkjenning. Større autonom agentarkitektur parkeres til bedre KI (> Opus 5).
