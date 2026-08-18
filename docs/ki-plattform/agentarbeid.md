# Agentarbeid i Pallur

Denne siden samler prinsipper og små eksperimenter for agentarbeid uavhengig av om modellen
kjører lokalt eller i skyen. Lokal KI-sporet er fortsatt parkert; agentharness kan vurderes
som et eget lag.

## Vurderingshorisont 2026-08-18

For læringen fra Sprytix-innlegget på X brukes to enkle kategorier akkurat i denne
vurderingen:

- **RIMELIG KI** — aktuelt å bruke eller prøve med dagens rimelige agenter.
- **BEDRE KI (> Opus 5)** — kan vente til agentene er vesentlig bedre enn dagens nivå.

Dette er ikke en generell klassifiseringsstandard for hele Pallur.

## Aktuelt for RIMELIG KI

### Git-worktrees for parallelle endringer

Når to agenter arbeider på samme repository, bør de normalt få hvert sitt Git-worktree og
sin egen gren. Det reduserer konflikt mellom filendringer og gjør resultatene enklere å
inspisere og forkaste.

Worktree er ikke en sikkerhetssandbox. Tilgang til shell, nettverk, credentials og andre
filer må fortsatt styres separat.

### Separate roller for implementering og kontroll

På en egnet, liten oppgave kan to rimelige agenter brukes slik:

1. **Implementeringsagent** analyserer oppgaven, gjør en liten endring og kjører relevante tester.
2. **Kontrollagent** får krav og diff med frisk kontekst og leter etter feil, mangler og unødvendig kompleksitet.
3. **Thor** vurderer diff og kontrollrapport før noe beholdes, committes eller pushes.

Poenget er ikke flest mulig agenter, men billig ekstra kontroll der den faktisk kan redusere
feilrisiko.

### Behold leverandørnøytral prosjektstyring

Pallur beholder `AGENTS.md` som inngang til `.kiitos/prosjektinstruks.md` og relevante
prosjektdokumenter. Det gir en harness- og modellnøytral variant av mønsteret der alle
agenter leser en felles prosjektinstruks først.

## Aktuelt når bedre KI kommer (> Opus 5)

Følgende ideer parkeres til agentene er vesentlig bedre:

- større parallelle agentteam eller «agentflåter»
- autonom orkestrering av mange samtidige løp
- delt persistent agentminne på tvers av mange arbeidsøkter
- knowledge graph som felles agentminne
- automatiske forbedringer av prosjektinstrukser basert på agentenes egne erfaringer

Selv med bedre KI skal endringer i `AGENTS.md`, Kiitos-instrukser og andre styringsfiler
som hovedregel foreslås som diff og godkjennes av menneske før de blir gjeldende.

## Minste eksperiment med RIMELIG KI

**Formål:** Finne ut om en separat kontrollagent gir nok kvalitetsgevinst til å forsvare
merkostnaden og ekstra koordinering.

**Oppsett:**

- velg én avgrenset kode- eller dokumentasjonsoppgave i et ufarlig repository
- opprett to separate worktrees/grener
- la agent A implementere
- la agent B kontrollere krav og diff med frisk kontekst
- ingen automatisk merge, commit eller push
- Thor tar endelig beslutning

**Mål:**

- fant kontrollagenten reelle feil eller forbedringer?
- ble sluttresultatet bedre enn med én agent?
- hvor mye ekstra tid og kostnad krevde kontrollen?
- hvor mye styring måtte Thor gjøre?
- var worktree-opplegget enkelt nok til å gjenta?

**Fortsett hvis:** kontrollagenten jevnlig finner nyttige ting og merkostnaden er liten.

**Stopp hvis:** agent nummer to hovedsakelig gjentar den første, skaper merarbeid eller gjør
arbeidsflyten merkbart tregere.

## Status

- Lokal modellkjøring: **parkert**, beslutningen fra 2026-06-30 står fast.
- To-agentmønster med worktrees: **lite eksperiment anbefalt**, ikke gjennomført ennå.
- Større autonom agentarkitektur: **parkert til bedre KI (> Opus 5)**.

## Idékilde

- Sprytix-innlegg på X, 2026: https://x.com/sprytixl/status/2089427496303292470

Innlegget brukes som idéinnspill, ikke som autoritativ dokumentasjon. Påstander om konkrete
produkter, ytelse eller leverandørpraksis må verifiseres mot primærkilder før de brukes som
fakta i Pallur.
