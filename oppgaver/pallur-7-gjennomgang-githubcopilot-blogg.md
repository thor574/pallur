## [pallur-7] Gjennomgå GitHub Copilot-bloggen og vurder implikasjoner for pallur-planen

- Oppdaget: 2026-06-30
- Kilde/sak: ønske om å holde pallur-planen oppdatert med utviklingen i skybasert AI-assistanse
- Prioritet: P3
- Estimat: S
- Status: ny
- Eier: thor

### Hva er observert
GitHub Copilot er den dominerende skybaserte AI-assistenten for utviklere og publiserer
faginnlegg på https://github.blog/ai-and-ml/github-copilot/ om agentmodus, modellvalg,
sikkerhet, MCP og arbeidsflyt. Pallur har et bevisst fokus på lokal og lukket
KI-kjøring, men Copilot-utviklingen er relevant som referanse og sammenligningspunkt.

### Foreslått retning
1. Les gjennom nylige innlegg på https://github.blog/ai-and-ml/github-copilot/.
2. Merk innlegg om agentmodus, MCP, sikkerhet, personvern og modellvalg.
3. Vurder for hver relevante artikkel: endrer dette noe i docs/00-plattformskisse.md,
   spesielt avsnittet om lukket vs. skybasert KI?
4. Vurder spesielt: er det sikkerhetsmessige eller funksjonelle argumenter i
   Copilot-miljøet som bør besvares eksplisitt i pallur-planen?
5. Oppdater relevante doc-filer eller parker oppfølgingsoppgaver ved funn.

### Risiko ved å vente
Pallur-planens begrunnelse for lokal KI over Copilot bør kunne møte de beste
argumentene for skybasert assistanse. Uten kjennskap til Copilot-utviklingen
kan begrunnelsen bli utdatert eller ufullstendig.

### Referanser
- https://github.blog/ai-and-ml/github-copilot/
- docs/00-plattformskisse.md
