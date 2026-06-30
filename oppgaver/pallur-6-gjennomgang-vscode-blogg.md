## [pallur-6] Gjennomgå VS Code-bloggen for AI-relaterte innlegg og vurder implikasjoner for pallur-planen

- Oppdaget: 2026-06-30
- Kilde/sak: ønske om å holde pallur-planen oppdatert med offisielle AI-funksjoner i VS Code
- Prioritet: P3
- Estimat: S
- Status: ny
- Eier: thor

### Hva er observert
VS Code publiserer faginnlegg om nye AI-funksjoner, agentmodus, utvidelsesAPI-er og
integrasjoner på https://code.visualstudio.com/blogs/. Pallur bruker VS Code som
primær IDE, men de offisielle AI-relaterte blogginnleggene er ikke gjennomgått
systematisk for pallur-planen.

### Foreslått retning
1. Les gjennom AI-relaterte innlegg på https://code.visualstudio.com/blogs/.
   (Søk etter kategorier som agent, Copilot, AI, MCP, chat.)
2. Merk innlegg som beskriver funksjoner eller mønstre relevante for pallur
   (innebygd agentmodus, MCP-støtte i VS Code, utvidelsesgrenser, sikkerhet).
3. Vurder for hver relevante artikkel: endrer dette noe i docs/00-plattformskisse.md,
   docs/10-v0.1-ollama-continue.md eller docs/20-v1-lm-studio-pi.md?
4. Vurder spesielt: dekker VS Codes innebygde AI-funksjoner noe av det Continue/Cline
   brukes til, og har det konsekvenser for valget av utvidelse?
5. Oppdater relevante doc-filer eller parker oppfølgingsoppgaver ved funn.

### Risiko ved å vente
VS Code har fått stadig mer innebygd AI-funksjonalitet. Uten oversikt over disse
kan pallur-planen overlappe med eller undervurdere hva som allerede finnes i editoren.

### Referanser
- https://code.visualstudio.com/blogs/
- docs/00-plattformskisse.md
- docs/10-v0.1-ollama-continue.md
