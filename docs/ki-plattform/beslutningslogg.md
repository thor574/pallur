# Beslutningslogg

Denne loggen skiller beslutninger fra hypoteser og åpne spørsmål. Nye beslutninger legges
øverst eller nederst etter samme tabellformat, med dato og status.

| ID | Dato | Beslutning | Status | Begrunnelse | Konsekvens |
|---|---|---|---|---|---|
| PALLUR-ADR-0001 | 2026-06-29 | Pallur dokumenterer KI-plattformen, mens Kiitos styrer arbeidsformen | Besluttet | Plattformkunnskap og arbeidsregler har ulikt eierskap | Generell plattformdokumentasjon legges i Pallur, ikke i Kiitos |
| PALLUR-ADR-0002 | 2026-06-29 | `v0.1` med Ollama og Continue beholdes som dokumentert mulighet | Besluttet | Sporet virket, men var tregt og manglet måling | Brukes som sammenligningsgrunnlag, ikke som hovedspor |
| PALLUR-ADR-0003 | 2026-06-29 | `v1` prøves med LM Studio som modellvert og Pi som agentharness | Foreløpig | Dette gir tydeligere skille mellom modellserver og agent | Krever lokal måling og arbeidsgiveravklaring før praktisk bruk |
| PALLUR-ADR-0004 | 2026-06-30 | Pallur parkeres inntil videre | Besluttet | 4 GB VRAM gir 3–5 tokens/sek — 4–10x tregere enn frontier-modeller. Continue kjøpt opp av Cursor. Ingen verktøy løser lokal embedding i kveld. | Gjenopptas ved: 16 GB VRAM tilgjengelig, gode modeller i < 4 GB VRAM, eller embedding viser seg å hjelpe |
| PALLUR-ADR-0004 | 2026-06-29 | Første dokumentasjonsstruktur legges under `docs/ki-plattform/` | Besluttet | Eksisterende `docs/` er enkel og bør ikke flyttes unødvendig | Ny inngang får samlet lokal-KI-stoff uten å bryte gamle lenker |
| PALLUR-ADR-0005 | 2026-06-29 | Ingen installasjon, commit, push eller praktisk Pi-pilot uten eksplisitt beskjed | Besluttet | Jobb-PC og agentverktøy krever kontroll | Første leveranse er dokumentasjon og plan, ikke maskinendring |
| PALLUR-ADR-0006 | 2026-06-30 | Embedding + RAG er nødvendig strategi på nåværende maskinvare | Besluttet | 4 GB VRAM gir for lav inferansehastighet uten embedding-støtte ved reelle kontekststørrelser | `nomic-embed-text-v1.5` aktiveres som primær kontekststrategi; ren LLM-inferanse krever ~16 GB VRAM |

## Åpne beslutninger

| Tema | Spørsmål | Før beslutning trengs |
|---|---|---|
| Første modell | Hvilken modell og kvantisering skal testes først i LM Studio? | Primærkilder, lisens, maskinbudsjett og lokal godkjenning |
| Pi-provider | Skal Pi bruke OpenAI completions- eller responses-kompatibilitet mot LM Studio? | Gjeldende Pi-schema og LM Studio-endepunkt må kontrolleres lokalt |
| Jobb-PC | Kan verktøyene brukes på arbeidsgiver-PC? | Arbeidsgivergodkjenning |
| Kiitos-pilot | Hva er første ufarlige oppgave mot `kiitos`? | Ren status, grenvalg og eksplisitt beskjed |
| Privat PC | Hvilke maskinvarekrav er reelle? | Besvart: 16 GB VRAM for ren LLM-inferanse; 4 GB VRAM krever embedding + RAG (PALLUR-ADR-0006) |

