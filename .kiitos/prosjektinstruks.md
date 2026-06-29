# Prosjektinstruks - pallur

Pallur dokumenterer KI-plattformen Thor bruker for agentarbeid, lokale modeller,
modellverter, IDE-integrasjon og kiitos-styring.

## Kiitos-flate

Når dette prosjektet åpnes som eget arbeidsrom, bruk denne leserekkefølgen:

1. `../kiitos.fyr/velkommen.md`
2. `../kiitos.laug.th/velkommen.md`
3. `../kiitos.ruff.th/velkommen.md`
4. `../kiitos.ruff.th/.github/copilot-kiitos-pref.md`
5. denne filen

Hvis katalogene ikke finnes ved siden av prosjektet, si tydelig fra før du gjør
prosjektnære vurderinger.

## Formål

Prosjektet skal beskrive:

- hvilke KI-verktøy som inngår i plattformen
- hvordan lokale modeller kjøres, måles og byttes
- hvordan agentharness, IDE-utvidelser og modellverter kobles sammen
- hva som er lukket/lokalt, og hva som eventuelt går mot skytjenester
- hvilke rutiner som gjelder for minne, VRAM, sikkerhet, journal og kildekontroll

## Versjoner

- `v0.1`: Ollama som lokal modellvert og Continue i VS Code.
- `v1`: LM Studio som lokal modellvert og Pi som terminalbasert agentharness.

`v0.1` er dokumentert som en reproduserbar mulighet, ikke som valgt normaloppsett.
`v1` er et prøvespor og må valideres lokalt før det kan løftes til anbefaling.

## Plassering

- Prosjektnær dokumentasjon ligger i `docs/`.
- Prosjektjournal ligger i `.kiitos/journal/`.
- Eksempelkonfigurasjon kan ligge i `examples/` når den ikke er hemmelig.
- Hemmeligheter, API-nøkler, tokens, auth-filer og maskinspesifikke private notater skal
  ikke sjekkes inn.

## Arbeidsregler

- Skriv norsk bokmål når ikke annet er avtalt.
- Skill mellom dokumentert erfaring, testet fakta og forslag som må verifiseres.
- Oppgi kilder når produktdokumentasjon eller eksterne verktøy beskrives.
- Ikke anta at modellnavn, endepunkter eller CLI-flagg er stabile uten å kontrollere dem.
- Bevar `æ`, `ø` og `å` i dokumentasjonen.
