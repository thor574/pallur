# Dokumentasjonsplan for lokal KI-plattform

## Orientering 2026-06-29

Funnet i Pallur:

- `README.md` med kort prosjektbeskrivelse og lenker til de første dokumentene.
- `AGENTS.md` som peker til `.kiitos/prosjektinstruks.md`.
- `.kiitos/prosjektinstruks.md` med plassering, formål og arbeidsregler.
- `docs/00-plattformskisse.md` med enkel arkitektur.
- `docs/10-v0.1-ollama-continue.md` med dokumentasjon av tidligere Ollama/Continue-spor.
- `docs/20-v1-lm-studio-pi.md` med første beskrivelse av LM Studio/Pi-spor.
- Prosjektjournal under `.kiitos/journal/`.
- `CODEX_START_PALLUR_KI_PLATTFORM.txt` som overleveringsfil fra ekstern samtale.

Funnet om Kiitos for senere pilot:

- `kiitos.fyr` eier universelle instrukser, plassering, journal og felles guider.
- `kiitos.laug.th` er privat arbeidsrom og peker prosjekter mot lokal `.kiitos/prosjektinstruks.md`.
- `kiitos.ruff.th` eier personlige preferanser og tidligere lokal-KI-notater.
- Første praktiske Pi-pilot mot `kiitos` skal ikke gjøres før den er eksplisitt godkjent.

Manglende i Pallur før denne planen:

- egen inngangsside for lokal KI-plattform
- tydelig skille mellom verifiserte fakta, planer og lokale hypoteser
- beslutningslogg
- eksperimentmal
- samlet synlighet for arbeidsgivergodkjenning og jobb-PC-risiko
- konkret plan for videre sider

Konvensjoner som bør følges:

- norsk bokmål
- korte Markdown-filer med relative lenker
- prosjektnære dokumenter under `docs/`
- prosjektjournal under `.kiitos/journal/`
- ingen hemmeligheter, maskinspesifikke private baner eller modellfiler i Git
- kilder og kontrollato når produktdokumentasjon brukes

## Første leveranse

Første leveranse skal være liten og navigerbar:

- opprette `docs/ki-plattform/README.md`
- dokumentere mål, avgrensning, arkitektur, sikkerhetsprinsipper og veikart
- opprette `docs/ki-plattform/beslutningslogg.md`
- opprette `docs/ki-plattform/eksperimentmal.md`
- oppdatere rot-README med lenke til den nye inngangen
- journalføre hva som ble gjort

## Senere dokumenter

Planlagte sider:

- `lm-studio.md`
- `pi-coding-agent.md`
- `hugging-face-og-modeller.md`
- `modellvalg-og-kvantisering.md`
- `maaling-og-eksperimenter.md`
- `arbeidsflyt-kiitos.md`
- `sikkerhet-jobb-pc.md`
- `feilsoking.md`
- `maskinvarebeslutning.md`

Eksempler som kan opprettes senere:

- `examples/pi/models.example.json`
- `examples/pi/project-instructions.example.md`

## Må verifiseres mot oppdaterte primærkilder

- LM Studio: serverstart, port, modell-ID, OpenAI-kompatibilitet, offline-modus og lagringssteder.
- Pi: installasjon, Windows-krav, `models.json`-schema, provider-API, verktøybegrensning,
  logging, historikk og avinstallasjon.
- Hugging Face: GGUF, LM Studio-integrasjon, lisensfelt, modellkort og nedlasting.
- Codex/OpenAI: gjeldende anbefalinger for `AGENTS.md`.

## Krever arbeidsgivergodkjenning

- installasjon eller bruk av LM Studio
- installasjon eller bruk av Pi, Node.js, npm eller tilsvarende runtime
- nedlasting av modeller fra Hugging Face
- bruk av lokal modell på arbeidsgiverkode
- åpning av lokal API-port, selv på loopback, dersom policy krever avklaring
- lagring av modellfiler, logger, samtalehistorikk og cache på arbeidsgiver-PC
- bruk av tredjeparts Pi-extensions, skills eller packages

## Neste anbefalte oppgave

Lag siden `lm-studio.md` med kontrollert, versjonsdatert dokumentasjon av:

- hva LM Studio er i Pallur-arkitekturen
- hva som må avklares før installasjon
- hvordan modellkort, lisens og GGUF-fil vurderes før nedlasting
- hvordan lokal server og modell-ID kontrolleres uten å anta at meny og CLI er uendret

