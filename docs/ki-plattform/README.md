# Lokal KI-plattform

Dette er inngangssiden for Pallurs dokumentasjon av en lokal, privat og kontrollerbar
KI-plattform for programvareutvikling.

Status per 2026-06-29: Dette er et prøvespor. Dokumentasjonen beskriver ønsket retning,
avgrensninger og måleopplegg. Den dokumenterer ikke at LM Studio, Pi eller nye modeller er
installert eller godkjent på jobb-PC.

## Brukermål (dokumentert 2026-06-30)

Intervju avdekket følgende konkrete mål for den lokale KI-plattformen:

**Ønsket funksjonalitet:**
- Chat om kode og kiitos — stille spørsmål og få forklaringer inne i VS Code
- Agent som kan lese filer, gjøre endringer og committe på mitt vegne
- *Ikke* prioritert: inline autocomplete mens man skriver

**Motivasjon for lokal kjøring:**
- Personvern — kode og kiitos skal ikke forlate maskinen
- Kostnad — unngå abonnement på sky-KI
- Jobb-PC-restriksjoner — skytjenester kan være begrenset

**Forhold til GitHub Copilot:**
- Supplement, ikke erstatning: sky til komplekse saker, lokalt til enklere og private oppgaver

**Akseptabel responstid:**
- Enkle forespørsler: ~5 sekunder
- Kompleks kode-refaktorisering: 3–10 minutter akseptabelt

## Plattformens lag

En lokal KI-plattform er ikke ett verktøy — det er flere uavhengige lag som må velges og
kobles sammen. Hvert lag har alternativer. Valgene i ett lag påvirker hva som er mulig i
et annet.

### Lag 1 — Modellvert

Programvaren som laster en KI-modell og eksponerer den som en lokal API-server.

| Kandidat | Merknad |
|---|---|
| **LM Studio** | Grafisk grensesnitt, god GPU-offload-kontroll, OpenAI-kompatibel server |
| **Ollama** | Kommandolinjeverktøy, enkel å sette opp, god modellhåndtering |
| **llama.cpp server** | Lavnivå, maksimal kontroll, krever mer manuell konfigurasjon |
| **Jan.ai** | Grafisk alternativ til LM Studio, enklere brukerflate |

Pallur v1 bruker LM Studio. Ollama ble testet i v0.1.

### Lag 2 — Modell

Selve KI-modellen som kjøres. Valget avgjøres av maskinens VRAM og RAM, lisens og oppgavetype.

| Hensyn | Forklaring |
|---|---|
| VRAM-budsjett | Avgjør hvor mange lag som kan ligge på GPU. 4 GB VRAM = maks ~7B Q4 i GPU. |
| RAM-budsjett | Resten av modellen legges i RAM. 64 GB gir god plass. |
| Kvantisering | Q4_K_M er vanligste avveining mellom størrelse og kvalitet. BF16 er full presisjon, mye større. |
| MoE-arkitektur | Mixture of Experts: stor total, men få aktive parametere per pass — rask inferanse. |
| Lisens | Avgjør om modellen kan brukes kommersielt, på jobb-PC og i hvilke kontekster. |

**GGUF-format**
Når du søker etter modeller på Hugging Face eller i LM Studio, vil du se filendelsen `.gguf`. GGUF
(GPT-Generated Unified Format) er et filformat for kvantiserte modeller utviklet av llama.cpp-prosjektet.
Det er i praksis blitt standarden for å kjøre åpne modeller lokalt: én enkelt fil som inneholder
både modellvektene og all nødvendig metadata. LM Studio, Ollama og llama.cpp kan alle laste GGUF-filer direkte.
Se [termer.md](termer.md) for ordforklaringer.

**Hvor finner man modeller?**
- [Hugging Face](https://huggingface.co) — det største åpne modellbiblioteket. Søk etter GGUF-varianter og les modelkortet for lisens, proveniens og kvantiseringsalternativer.
- LM Studio sin innebygde modellvisning — henter fra Hugging Face, men viser minneindikator for maskinen din direkte.

Se [lm-studio.md](lm-studio.md) for modellene som er tilgjengelige lokalt.

### Lag 3 — Indeksering og konteksthenting (RAG)

En 9B-modell har ikke plass til en hel kodebase i kontekstvinduet. Løsningen er å
indeksere dokumenter og kode med en embedding-modell og lagre vektorene i en lokal
database, slik at bare relevante utdrag sendes til modellen per spørring.

```text
Dokumenter / kode
  → embedding-modell → 768-dimensjonale vektorer
  → vektordatabase (lagrer og søker)

Ved spørsmål:
  → embed spørsmålet → søk i databasen → hent relevante utdrag → send til LLM
```

| Komponent | Kandidater |
|---|---|
| Embedding-modell | `nomic-embed-text-v1.5` (tilgjengelig lokalt via LM Studio) |
| Vektordatabase | ChromaDB (Python), LanceDB (Python/Node), Qdrant (Docker) |

Uten dette laget vil en lokal 9B-modell på 4 GB VRAM være for treg ved lange arbeidsøkter.

**Merk — VS Code har innebygget semantisk indeksering:**
VS Code indekserer arbeidsrommet ditt løpende og tilbyr et `semantic_search`-verktøy internt.
GitHub Copilot får bruke denne indekseringen direkte, og slipper derfor å bygge sin egen
vektordatabase. For lokale KI-modeller er denne indekseringen ikke tilgjengelig — den er
intern i VS Code og ikke eksponert som et åpent API. Derfor må vi bygge et tilsvarende
lag selv med embedding-modell og vektordatabase.

### Lag 4 — Brukerflate og agentsele (eng: harness)

Det du faktisk bruker: der du skriver spørsmål, ser svar og lar agenten gjøre endringer.
Se [termer.md](termer.md) for forklaring av begrepet agentsele.

| Kandidat | Type | Merknad |
|---|---|---|
| **Cline** | VS Code-utvidelse | Chat + agent, lese/skrive filer, committe, åpen kildekode |
| **Roo Code** | VS Code-utvidelse | Fork av Cline med ekstra funksjoner |
| **Continue** | VS Code-utvidelse | Utvikling i praksis stoppet — se under |
| **Pi Coding Agent** | Terminal | Kjenner `AGENTS.md`, OpenAI-kompatibel, krever bash på Windows |
| **GitHub Copilot** | VS Code / sky | Referansepunkt — lukket, skybasert, god opplevelse |

**Om Continue.dev (oppdatert 2026-06-30):**
Continue.dev er ikke lenger aktivt vedlikeholdt. Hovedutvikleren ble ansatt i Cursor og
arbeider ikke videre med Continue. Prosjektet eksisterer fortsatt og v1 kan brukes, men
bør ikke velges som langsiktig plattform uten at ny vedlikeholder trer til. Cline og
Roo Code er per nå mer aktive alternativer.

Alle kan konfigureres mot LM Studio sin lokale server som backend.

### Lag 5 — Trygt verktøyvalg

Å velge riktig teknisk løsning er ikke nok. Verktøyet må også brukes forsvarlig.

**Steg 1 — Eksperimenter på ikke-kritiske data**
Prøv verktøyet på ufarlige prosjekter uten sensitiv kode, persondata eller
forretningshemmeligheter. Bekreft at det virker som forventet.

**Steg 2 — Verifiser at private data ikke lekker**
Sjekk at modellen ikke sender data til eksterne tjenester. LM Studio kjører lokalt, men
sjekk nettverkstrafikk ved mistanke. Les dokumentasjonen til hvert verktøy.

**Steg 3 — Innhent tillatelse fra arbeidsgiver**
Før rutinemessig bruk på jobb-PC eller med jobbrelatert kode: få eksplisitt godkjenning.
Dette gjelder særlig agentverktøy som kan lese og endre filer.

---

## Mål

Pallur skal gjøre det mulig å forstå, prøve og senere velge en praktisk KI-plattform for
agentstøttet utvikling.

Nåværende spor (v1):

```text
Hugging Face
  -> modellkort, lisens og GGUF-modellfil
LM Studio på lokal Windows-PC  [Lag 1 + 2]
  -> lokal OpenAI-kompatibel API-server
Embedding + RAG (planlagt)     [Lag 3]
  -> nomic-embed-text-v1.5 + vektordatabase
Brukerflate (under vurdering)  [Lag 4]
  -> Cline eller Pi i VS Code / terminal
Git
  -> diff, historikk og kontroll
```

IDE-en (VS Code eller IntelliJ IDEA) er fortsatt normal arbeidsflate for lesing,
debugging, tester og manuell utvikling.

## Avgrensning

Pallur skal ikke være et sted for:

- arbeidsgiverkode, produksjonsdata, personopplysninger eller hemmeligheter
- ekte tokens, API-nøkler eller private konfigurasjonsfiler
- modellfiler eller store cache-kataloger
- generelle kiitos-regler som hører hjemme i `kiitos.fyr`
- praktiske kodeendringer i `kiitos`

Pallur skal i stedet dokumentere arkitektur, sikkerhet, modellvalg, målinger, forsøk og
beslutninger.

## Verifiserte fakta og planer

Verifisert mot primærkilder 2026-06-29:

- LM Studio dokumenterer lokal server og OpenAI-kompatible endepunkter.
- LM Studio dokumenterer offline bruk etter at nødvendige modeller og runtimes finnes lokalt.
- Pi dokumenterer egendefinerte modelltilbydere i `~/.pi/agent/models.json`.
- Pi dokumenterer Windows-oppsett med bash-krav.
- Pi dokumenterer at verktøyet ikke har innebygget sandbox.
- Hugging Face dokumenterer GGUF og bruk av LM Studio med Hub-modeller.
- Codex dokumenterer bruk av `AGENTS.md` som repositoryinstruks.

Ikke verifisert lokalt ennå:

- nøyaktig LM Studio-versjon på maskinen
- nøyaktig Pi-versjon og faktisk schema i lokal installasjon
- hvilke modeller som er tillatt, raske nok og gode nok
- faktisk RAM-, VRAM- og sidevekslingsbruk
- om arbeidsgiver godkjenner verktøyene og bruken

## Sikkerhetsprinsipper på jobb-PC

Arbeids-PC-en er arbeidsgiverstyrt. Dokumentasjonen skal derfor skille mellom:

- teknisk mulig
- sikkerhetsmessig forsvarlig
- faktisk tillatt

Minimumsprinsipper for prøving:

- bind lokal modellserver til `127.0.0.1` eller `localhost`
- ikke eksponer API-et på lokalnett eller internett
- ikke bruk arbeidsgiverkode eller arbeidsdata uten eksplisitt godkjenning
- ikke legg hemmeligheter i repository, prompt, modellkonfigurasjon eller logger
- start med Pallur eller et ufarlig testrepository
- la agenten undersøke før den endrer
- begrens filer og verktøy i de første forsøkene
- gjennomgå `git diff` før endringer beholdes
- ikke commit, push, publiser eller installer automatisk

## Forholdet mellom Pallur og Kiitos

Pallur dokumenterer plattformen. Kiitos styrer arbeidsformen.

Pallur kan beskrive hvordan en senere Pi-pilot mot `kiitos` bør gjøres, men første
praktiske pilot mot `kiitos` skal være liten, reverserbar og eksplisitt godkjent.

Foreløpig pilotregel:

1. ren Git-status
2. separat gren etter eksplisitt beskjed
3. liten dokumentasjonsoppgave
4. Pi undersøker før redigering
5. menneskelig diff-gjennomgang
6. resultat journalføres i Pallur
7. ingen automatisk commit eller push

## Veikart

| Fase | Leveranse | Status |
|---|---|---|
| 1 | Inngangsside, mål, arkitektur, sikkerhet, plan, beslutningslogg og eksperimentmal | Påbegynt |
| 2 | Dypere sider for LM Studio, Pi, Hugging Face, modellvalg og måling | Planlagt |
| 3 | Første lokale måling i Pallur eller ufarlig testrepo | Krever lokal godkjenning |
| 4 | Første Pi-pilot mot `kiitos` | Krever eksplisitt beskjed |
| 5 | Maskinvarevurdering for eventuell privat PC | Senere |

## Videre dokumenter

- [Dokumentasjonsplan](dokumentasjonsplan.md)
- [Beslutningslogg](beslutningslogg.md)
- [Eksperimentmal](eksperimentmal.md)
- [Plattformskisse](../00-plattformskisse.md)
- [v0.1: Ollama og Continue](../10-v0.1-ollama-continue.md)
- [v1: LM Studio og Pi](../20-v1-lm-studio-pi.md)

## Primærkilder

- [LM Studio docs](https://lmstudio.ai/docs/)
- [LM Studio local server](https://lmstudio.ai/docs/developer/core/server)
- [LM Studio OpenAI compatibility](https://lmstudio.ai/docs/developer/openai-compat)
- [LM Studio offline use](https://lmstudio.ai/docs/app/offline)
- [Pi docs](https://pi.dev/docs/latest)
- [Pi custom models](https://pi.dev/docs/latest/models)
- [Pi security](https://pi.dev/docs/latest/security)
- [Pi Windows setup](https://pi.dev/docs/latest/windows)
- [Hugging Face: LM Studio](https://huggingface.co/docs/hub/lmstudio)
- [Hugging Face: GGUF](https://huggingface.co/docs/hub/gguf)
- [OpenAI Codex: AGENTS.md](https://developers.openai.com/codex/guides/agents-md)

