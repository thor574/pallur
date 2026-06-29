# Lokal KI-plattform

Dette er inngangssiden for Pallurs dokumentasjon av en lokal, privat og kontrollerbar
KI-plattform for programvareutvikling.

Status per 2026-06-29: Dette er et prøvespor. Dokumentasjonen beskriver ønsket retning,
avgrensninger og måleopplegg. Den dokumenterer ikke at LM Studio, Pi eller nye modeller er
installert eller godkjent på jobb-PC.

## Mål

Pallur skal gjøre det mulig å forstå, prøve og senere velge en praktisk KI-plattform for
agentstøttet utvikling.

Første hovedspor er:

```text
Hugging Face
  -> modellkort, lisens og GGUF-modellfil
LM Studio på lokal Windows-PC
  -> lokal OpenAI-kompatibel API-server
Pi Coding Agent
  -> undersøker og endrer et avgrenset Git-repository
Git
  -> diff, historikk og kontroll
```

IDE-en, for eksempel IntelliJ IDEA eller VS Code, er fortsatt normal arbeidsflate for
lesing, debugging, tester og manuell utvikling.

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

