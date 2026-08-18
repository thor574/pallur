# Agentarbeid med rimelig KI

## Bakgrunn

Et Sprytix-innlegg på X om parallelle agentløp, Git-worktrees, felles instruks og delt
agentminne ble vurdert for Pallur.

Thor ønsket denne gangen et enkelt skille:

- `RIMELIG KI`: aktuelt nå med dagens rimelige agenter
- `BEDRE KI (> Opus 5)`: kan vente til agentene er vesentlig bedre

## Beslutning

Lokal KI-sporet fra 2026-06-30 forblir parkert. Agentharness vurderes separat.

Aktuelt for `RIMELIG KI`:

- separate Git-worktrees ved parallelle endringer
- én implementeringsagent og én kontrollagent på små oppgaver
- frisk kontekst for kontrollagenten
- menneskelig gjennomgang av diff
- ingen automatisk merge, commit eller push

Parkert til `BEDRE KI (> Opus 5)`:

- større agentteam eller agentflåter
- autonom orkestrering av mange løp
- persistent delt agentminne
- knowledge graph som agentminne
- selvmodifiserende prosjektinstrukser

Selv med bedre KI skal endringer i styringsfiler normalt foreslås som diff og godkjennes av
menneske.

## Endringer

- `docs/ki-plattform/agentarbeid.md` opprettet med klassifisering og minste eksperiment.
- `docs/ki-plattform/beslutningslogg.md` oppdatert med beslutningene.
- `README.md` oppdatert slik at lokal KI og agentharness har separat status.

## Status

Det anbefalte to-agentforsøket er dokumentert, men ikke gjennomført. Neste praktiske steg er
først aktuelt når en liten og ufarlig oppgave egner seg for forsøket.
