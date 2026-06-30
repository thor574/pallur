# KI-instruks 1
> Bruk kiitos som styringsgrunnlag. Det er en samling instrukser i tre katalogtrær. Gi beskjed hvis du ikke finner dem.

Kiitos-flaten ble funnet ved siden av prosjektet:

- kiitos.fyr
- kiitos.laug.th
- kiitos.ruff.th

kiitos.arkiv finnes også, men ble ikke brukt som aktiv instruksflate.



# KI-instruks 2
> Hei, dette er et nytt prosjekt, som egentlig skal bli en beskrivelse av den KI - plattformen jeg bruker.  Vi kan tenke på det som v1 pallur, fordi dette kan endre seg over tid.
>
> Jeg har til nå (v0.1) prøvd en enkel løsning; Ollama som kjører lokale KI modeller, og har brukt Continue extension i VSC for å få tak i den, og det virket, men tregt.  Det brukte sikkert RAM, og opplegget var veldig enkelt og laget uten altfor stor innsikt.
>
> Jeg så nå en video i youtube og ble fascinert, for fyren forklarte at man kan legge en del av modellen i VRAM (han hadde selv 16 GB), helst alt, og resten kan ligge i RAM.  Han brukte LM Studio og pi.dev.  Det virket inspirende for meg til å prøve dette nå, i det som kan bli v1 pallur.
>
> Hvis du kjenner til detaljene om hva jeg gjorde med Ollama og Continue, legg det inn som en dokumentasjon på en mulighet.  Hvis du kjenner til hvordan gjøre det med LM Studio og pi.dev, legg det inn i en annen dokumentasjonsfil.  Hvis du ikke kjenner til dette, så gi beskjed, så skal du få instrukser fra en logg.
>
> Vi bruker kiitos mens vi jobber med prosjekter.

## Problem: Oppstart av Pallur v1

Prosjektet ble startet som dokumentasjonsprosjekt for KI-plattformen Pallur. Eksisterende
kiitos-ruff-notater om Ollama og Continue ble brukt som kilde for `v0.1`. Offentlig
dokumentasjon for LM Studio og Pi ble kontrollert 2026-06-29 før `v1`-sporet ble skrevet.

### Endringer utført

- `AGENTS.md` ble opprettet som tynn inngang til lokal kiitos-instruks.
- `.kiitos/prosjektinstruks.md` ble opprettet som prosjektnær styring.
- `README.md` ble utvidet med formål, dokumentasjonsindeks og status.
- `docs/00-plattformskisse.md` ble opprettet som kort oversikt.
- `docs/10-v0.1-ollama-continue.md` ble opprettet med dokumentasjon av Ollama/Continue-sporet.
- `docs/20-v1-lm-studio-pi.md` ble opprettet med foreslått LM Studio/Pi-spor og kilder.

### Status

Dokumentasjonen er opprettet som første versjon. LM Studio/Pi-sporet er ikke testet lokalt
ennå og er derfor merket som prøvespor.

### Kostnader

Kostnadstall var ikke tilgjengelige i denne Codex-sesjonen. Feltet må oppdateres senere
hvis Pallur skal føre konkret økonomisporing for analyse, design, implementasjon eller
dokumentasjon.



# KI-instruks 3
> Les CODEX_START_PALLUR_KI_PLATTFORM.txt i sin helhet og utfør første oppdrag i filen.
>
> (Dette var beskjeden fra din kollega ChatGPT 5.5 (Høy) når jeg ba den sammenfatte vår tråd der, så vi kunne fortsette her, og det blir enklere for meg, fordi du kan lage filer og vedlikeholde dem over tid, pluss fagfellevurderingen jeg gjør av arbeidet ditt er enklere her).

## Problem: Første leveranse for lokal KI-plattform

`CODEX_START_PALLUR_KI_PLATTFORM.txt` ble lest i sin helhet. Oppdraget ble gjennomført
primært i Pallur. Kiitos ble bare lest for å forstå senere pilot og ble ikke endret.

### Endringer utført

- `README.md` ble oppdatert med lenke til lokal KI-plattform.
- `docs/ki-plattform/README.md` ble opprettet som inngangsside med mål, avgrensning,
  arkitektur, sikkerhetsprinsipper, Pallur/Kiitos-skille og veikart.
- `docs/ki-plattform/dokumentasjonsplan.md` ble opprettet med orientering, første
  leveranse, senere sider, primærkilder og arbeidsgiveravklaringer.
- `docs/ki-plattform/beslutningslogg.md` ble opprettet med første beslutninger og åpne
  beslutningspunkter.
- `docs/ki-plattform/eksperimentmal.md` ble opprettet som mal for repeterbare målinger.

### Status

Første leveranse er gjennomført som dokumentasjon og plan. Ingen programvare ble installert,
ingen endringer ble gjort i `kiitos`, og ingen commit eller push ble utført.

### Kostnader

Kostnadstall var ikke tilgjengelige i denne Codex-sesjonen.



# KI-instruks 4
> Nydelig.
>
> Kanskje vi nå kan slette CODEX_START*.txt filen.
> Er det tydelig hva neste steg er?
>
> Journalfør og commit.

## Problem: Opprydding etter overleveringsfil

Overleveringsfilen `CODEX_START_PALLUR_KI_PLATTFORM.txt` var ferdig brukt og var tracked
i siste commit. Den ble derfor slettet fra Pallur.

### Status

Neste steg er tydelig i `docs/ki-plattform/dokumentasjonsplan.md`: lag siden
`lm-studio.md` med kontrollert, versjonsdatert dokumentasjon av LM Studio sin rolle,
installasjonsavklaringer, modellkort/lisens/GGUF-vurdering og kontroll av lokal server
og modell-ID.

Arbeidet journalføres og committes i Pallur.

### Kostnader

Kostnadstall var ikke tilgjengelige i denne Codex-sesjonen.
