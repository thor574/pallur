## [pallur-3] Gjennomgå Ollama-bloggen og vurder implikasjoner for pallur-planen

- Oppdaget: 2026-06-30
- Kilde/sak: ønske om å holde pallur-planen oppdatert med beste praksis fra lokal modellverting
- Prioritet: P2
- Estimat: S
- Status: ny
- Eier: thor

### Hva er observert
Ollama er den lokale modellverten brukt i pallur v0.1 og dekker emner som modellformat,
VRAM-styring, API-konfigurasjon og integrasjoner i sin blogg på https://ollama.com/blog.
Pallur-planen beskriver Ollama-oppsettet, men artiklene er ikke systematisk gjennomgått
for oppdaterte anbefalinger.

### Foreslått retning
1. Les gjennom artiklene på https://ollama.com/blog.
2. Merk artikler som er relevante for pallur (modellvalg, VRAM, API-oppsett,
   integrasjon med Continue/Cline, ytelse og sikkerhet).
3. Vurder for hver relevante artikkel: endrer dette noe i docs/00-plattformskisse.md
   eller docs/10-v0.1-ollama-continue.md?
4. Oppdater relevante doc-filer eller parker oppfølgingsoppgaver ved funn.

### Risiko ved å vente
Ollama er i rask utvikling. Anbefalte modeller, konfigurasjonsmønstre og
sikkerhetsråd kan ha endret seg siden docs/10-v0.1-ollama-continue.md ble skrevet.

### Referanser
- https://ollama.com/blog
- docs/00-plattformskisse.md
- docs/10-v0.1-ollama-continue.md
