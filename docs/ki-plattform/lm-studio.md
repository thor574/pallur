# LM Studio

LM Studio er modellverten i Pallur v1. Den kjører GGUF-modeller lokalt, eksponerer en
OpenAI-kompatibel API-server og gir grafisk kontroll over GPU-avlasting, kontekststørrelse
og inferanseparametere.

Kildet fra offentlig LM Studio-dokumentasjon og lokal serverkontroll 2026-06-30.

## Rolle i arkitekturen

```text
LM Studio (localhost:1234)
  <- Pi Coding Agent (agentharness)
  <- VS Code / annet klientverktøy
  <- lokal PowerShell-test
  -> GGUF-modell lastet fra Hugging Face
```

## Lokal server

Serveren startes i Developer-fanen i LM Studio, eller via:

```powershell
lms server start
```

Standard port er `1234`. Kontroller at serveren er oppe:

```powershell
Invoke-RestMethod http://localhost:1234/v1/models | ConvertTo-Json -Depth 5
```

## Tilgjengelige modeller (kontrollert 2026-06-30)

Følgende modeller ble funnet ved direkte API-spørring mot kjørende lokal server:

| Modell-ID | Type | Kvantisering | Vurdering |
|---|---|---|---|
| `qwythos-9b-claude-mythos-5-1m@q4_k_m` | 9B, tilpasset Mythos 5 | Q4_K_M | Anbefalt for testing — passer delvis i VRAM |
| `qwythos-9b-claude-mythos-5-1m@bf16` | 9B, tilpasset Mythos 5 | BF16 (full) | ~18 GB — kjøres i RAM, unngå som primær |
| `qwen/qwen3.5-9b` | 9B grunnmodell | ukjent | Basismodell — Qwythos Q4 er bedre startpunkt |
| `qwen/qwen3.6-35b-a3b` | MoE 35B / 3B aktive | ukjent | Se under |
| `text-embedding-nomic-embed-text-v1.5` | Embedding | — | Aktuell for fremtidig RAG/søk |

### Qwythos-9B-Claude-Mythos-5

Qwythos er en community-modell bygget på Qwen3.5-9B og tilpasset Mythos 5-sporing.
Modellen annonserer Vision, Tool-use og Reasoning, og støtter opptil 1 048 576 tokens
kontekstvindu.

**Lisens og proveniens:** Må avklares mot modelkortet på Hugging Face før fast bruk.
GGUF-utgiver skal noteres når modellen er lastet ned.

### Qwen3.6-35B-A3B

`a3b` betyr Mixture of Experts (MoE) med ca. 3 milliarder aktive parametere per
token-pass, selv om totalmodellen er 35B. Dette gir:

- Inferansehastighet nær en 3B-modell
- Kvalitet nærmere en fullstor modell
- Krav om at alle lag (35B) ligger i RAM eller VRAM

For denne maskinen (4 GB VRAM, 64 GB RAM): modellen lastes i RAM med eventuell delvis
GPU-avlasting. Verdt å sammenligne mot Qwythos Q4 på samme testoppgave.

## Maskinvarebudsjett

Se [arbeidsstasjon-2025.md](../arbeidsstasjon-2025.md) for full maskinvarebeskrivelse.

Nøkkeltall for modellasting:

| Komponent | Kapasitet | KI-relevans |
|---|---|---|
| VRAM (RTX 500 Ada) | 4 GB | Begrenser antall lag som kan offloades til GPU |
| RAM | 64 GB | God plass til hele modeller i RAM ved CPU-inferanse |
| CPU | Intel Core Ultra 7 155H, 22 tråder | Brukbar CPU-inferanse på 7–9B-modeller |

### Praktisk ytelsesgrense — målt 2026-06-30

Forsøk med Qwythos-9B Q4 og kontekststørrelser opp mot 101 K tokens viste at responshastigheten
er for lav for praktisk iterativt arbeid uten embedding-støtte.

**Funn:** 4 GB VRAM er ikke tilstrekkelig for å kjøre en 9B-modell med akseptabel hastighet
når konteksten vokser. Mesteparten av inferansen faller tilbake på CPU og RAM.

**Estimert VRAM-behov for ok ytelse uten embedding:** ca. 16 GB VRAM ville gitt rom for
full GPU-inferanse på en 9B Q4-modell med romslig KV-cache. Med 4 GB VRAM er
embedding + RAG den realistiske veien til brukbar ytelse på denne maskinen.

## Konfigurasjonsanbefalinger

Basert på maskinbudsjettet og gjennomgang 2026-06-30.

### GPU Offload

- 9B Q4-modell (~5–6 GB): start med 16–20 lag offloaded. Øk om VRAM-målinger tillater det.
- BF16-varianter (~18 GB): minimalt GPU-offload, kjøres i RAM.
- MoE 35B: prøv 4–8 lag offloaded, resten i RAM.

Sjekk VRAM-bruk i Windows Task Manager (GPU Memory) etter lasting.

### Context Length

Modellen støtter opp til 1 048 576 tokens, men KV-cachen vokser lineært med
kontekststørrelsen og belaster RAM kraftig.

**Lokal modell vs. sky:** I arbeid med Claude Sonnet 4.5+, Opus 4.5+ og GPT 5.4+ er det
realistisk at en enkelt arbeidsøkt vokser fra ~20K til 150–200K tokens over 20 iterasjoner,
særlig ved vedlikehold av kode og kiitos. Skytjenestene håndterer dette med stor infrastruktur
og forbedret kontekstkompresjon. En lokal 9B-modell er ikke tilsvarende:

- KV-cache for 150K+ tokens sprenger VRAM og legger seg tungt i RAM
- Inferanse med full kontekst blir vesentlig tregere
- 9B-modeller har generelt svakere evne til å utnytte svært lang kontekst

**Praktisk strategi for lokalt:**

| Kontekst | Situasjon |
|---|---|
| 16 384 (16K) | Korte, avgrensede spørsmål — rask respons |
| 32 768 (32K) | Kodegjennomgang av én fil eller avgrenset sak |
| 65 536 (64K) | Bevisst valg ved mellomstore dokumenter |
| 101 010+ | Eksperiment — forvent tregere respons og mer RAM-press |

For lange iterative arbeidsøkter der konteksten vokser, er det bedre å bruke
embedding-modellen (`nomic-embed-text-v1.5`) til RAG: hent bare relevant kontekst per
spørring fremfor å mate hele historikken inn i modellen. Se planlagt side
`hugging-face-og-modeller.md` og `maaling-og-eksperimenter.md`.

## Embedding og RAG som erstatning for lang kontekst

Den sentrale utfordringen på denne maskinen er at 4 GB VRAM ikke er nok til å holde en
9B-modell i GPU ved lange kontekster. Løsningen er å kombinere lokal LLM med en
embedding-modell og RAG (Retrieval Augmented Generation).

### Prinsipp

I stedet for å sende hele kodebasen eller hele samtalehistorikken til modellen, gjøres dette:

```text
Kodebas / dokumenter / journal
  -> chunkes i mindre biter
  -> embedding-modell lager vektorer per bit
  -> vektorer lagres i lokal vektordatabase

Ved spørring:
  -> spørsmålet embeds
  -> nærmeste biter hentes (semantisk søk)
  -> bare de relevante bitene sendes til LLM
  -> LLM svarer med liten, presis kontekst
```

### Embedding-modell tilgjengelig

`text-embedding-nomic-embed-text-v1.5` er allerede lastet i LM Studio og eksponert via
serveren. Den kan brukes direkte via:

```powershell
$body = @{
    model = "text-embedding-nomic-embed-text-v1.5"
    input = "teksten som skal embeds"
} | ConvertTo-Json

Invoke-RestMethod -Method Post `
    -Uri http://localhost:1234/v1/embeddings `
    -ContentType "application/json" `
    -Body $body
```

### Konsekvens for plattformvalg

- Embedding er ikke valgfri tilleggsfunksjon — det er den primære mekanismen for å
  gjøre lokal inferanse brukbar ved reelle arbeidsøkter på denne maskinen.
- Ønsker man ren LLM-inferanse uten RAG med akseptabel hastighet, kreves ca. **16 GB VRAM**.
- Embedding + RAG er riktig valg gitt nåværende maskinvare.

### Flash Attention

Slå på. Reduserer minneforbruk ved lange kontekster og støttes av de fleste nyere GGUF-runtimes.

### CPU Thread Pool Size

Med 22 tråder tilgjengelig kan `CPU Thread Pool Size` settes til 12–16 for bedre
CPU-inferanseytelse. Standard 8 er konservativt.

### Draft MTP (Multi-Token Prediction)

ON med Max Draft Tokens: 2 er et godt utgangspunkt. Gir raskere generering via spekulativ
dekoding uten vesentlig risiko for feil.

### Unified KV Cache

Experimental. Kan gi bedre minnehåndtering. Slå av hvis du opplever stabilitetsproblemer.

## Åpne punkter

| Spørsmål | Status |
|---|---|
| Nøyaktig LM Studio-versjon på maskinen | Ikke notert |
| Lisens for Qwythos/Mythos 5 | Ikke kontrollert |
| GGUF-utgiver for Qwythos | Ikke notert |
| Lisens for Qwen3.6-35B-A3B | Ikke kontrollert |
| Godkjenning for bruk på jobb-PC | Ikke avklart |
| Faktisk VRAM-forbruk ved ulike lag-innstillinger | Ikke målt |
| Sammenligning Qwythos Q4 vs. Qwen3.6 MoE | Ikke gjennomført |
