# Pallur v1 - plattformskisse

Pallur er arbeidsnavnet på en KI-plattform for styrt arbeid med agenter, kode, lokale
modeller og prosjektkunnskap.

## Målbilde

```text
Kiitos
`-- styringsgrunnlag, prosjektinstruks, journal og arbeidsregler

Agentharness
`-- Codex, Pi, Continue eller annet verktøy som styrer samtale, filer og verktøy

Modellvert
`-- Ollama eller LM Studio som kjører modeller lokalt

Modeller
`-- kode-, samtale-, embedding- og eventuelt multimodale modeller
```

## Versjonsspor

| Spor | Modellvert | Brukerflate | Status | Hovedidé |
|---|---|---|---|---|
| `v0.1` | Ollama | Continue i VS Code | Dokumentert mulighet | Lukket, enkelt og lokalt, men tregt i første forsøk |
| `v1` | LM Studio | Pi i terminal | Prøvespor | Bedre kontroll med modelllasting, VRAM-avlasting og agentharness |

## Prinsipper

- Lokal først: kode og prosjektkontekst skal ikke sendes ut når målet er lukket drift.
- Mål før valg: kaldstart, varmstart, tokenhastighet, minne, VRAM og sideveksling må måles.
- Små modeller til hyppige oppgaver: autocomplete og enkle kodeforslag skal ikke bruke stor modell.
- Én stor modell om gangen: hold normalt bare én tung modell aktiv sammen med eventuell liten hjelpe- eller fullføringsmodell.
- Synlige endringer: agentverktøy skal gi diff, journal eller annen sporbarhet før endringer regnes som ferdige.
- Kiitos styrer prosjektarbeidet: lokale prosjektinstrukser, journal og kilder holdes oppdatert.

## Foreløpig beslutning

`v1` skal prøves som et eget spor med LM Studio og Pi. Det er ikke valgt som endelig
normaloppsett før det er målt mot `v0.1` på samme maskin og samme representative oppgave.
