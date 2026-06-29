# v1 - LM Studio og Pi

Dette er et foreslått prøvespor for Pallur v1. Innholdet er kontrollert mot offentlig
produktdokumentasjon 2026-06-29, men er ikke testet lokalt i dette prosjektet ennå.

## Kortversjon

```text
Pi
`-- OpenAI-kompatibel provider i ~/.pi/agent/models.json
    `-- http://localhost:1234/v1
        `-- LM Studio lokal server
            `-- nedlastet lokal modell med valgt kontekst og GPU-avlasting
```

LM Studio blir modellvert. Pi blir terminalbasert agentharness som leser `AGENTS.md`,
bruker prosjektfiler og kan kobles mot en lokal OpenAI-kompatibel modellserver.

## Hvorfor dette er interessant

LM Studio har innebygget modellnedlasting, lokal server og OpenAI-kompatible endepunkter.
Dokumentasjonen beskriver lokal server via Developer-fanen eller `lms server start`, typisk
på port `1234`.

LM Studio kan også lagre per-modell-innstillinger, blant annet GPU offload, kontekststørrelse
og Flash Attention. Det passer bedre til forsøket med å legge mest mulig av modellen i VRAM
og la resten gå i RAM.

Pi er et lite terminalbasert agentharness. Det støtter `AGENTS.md`, prosjektkontekst,
sessions, slash-kommandoer og egendefinerte modeller via `~/.pi/agent/models.json`.

## Forutsetninger

- Windows-maskin med nok RAM og helst dedikert GPU/VRAM.
- Git Bash tilgjengelig, fordi Pi krever bash på Windows.
- LM Studio installert.
- Node/npm, pnpm eller annet støttet installasjonsverktøy for Pi.
- Avklart at verktøyene er tillatt på aktuell maskin.

## LM Studio-oppsett

1. Installer LM Studio.
2. Last ned en modell som passer maskinen.
3. Velg en kvantisert variant som faktisk får plass i minnebudsjettet.
4. Sett per-modell-standarder før fast bruk:
   - GPU offload: start høyt, men reduser hvis VRAM går fullt.
   - Context size: start moderat, for eksempel 16K eller 32K.
   - Flash Attention: prøv når runtime og modell støtter det.
5. Start lokal server i Developer-fanen eller med:

```powershell
lms server start
```

6. Kontroller server:

```powershell
Invoke-RestMethod http://localhost:1234/v1/models
```

## Pi-oppsett

Installer Pi med npm:

```powershell
npm install -g --ignore-scripts @earendil-works/pi-coding-agent
```

På Windows må Pi finne bash. Hvis Git Bash ikke ligger på standardplass, sett `shellPath`
i `~/.pi/agent/settings.json`.

Eksempel på lokal LM Studio-provider i `~/.pi/agent/models.json`:

```json
{
  "providers": {
    "lmstudio": {
      "baseUrl": "http://localhost:1234/v1",
      "api": "openai-completions",
      "apiKey": "lm-studio",
      "compat": {
        "supportsDeveloperRole": false,
        "supportsReasoningEffort": false
      },
      "models": [
        {
          "id": "bruk-modell-id-fra-lm-studio",
          "name": "LM Studio lokal modell",
          "reasoning": false,
          "input": ["text"],
          "contextWindow": 32768,
          "maxTokens": 4096,
          "cost": {
            "input": 0,
            "output": 0,
            "cacheRead": 0,
            "cacheWrite": 0
          }
        }
      ]
    }
  }
}
```

Start i prosjektmappen:

```powershell
pi --provider lmstudio --model bruk-modell-id-fra-lm-studio
```

Alternativt:

```powershell
pi --model lmstudio/bruk-modell-id-fra-lm-studio
```

## Kiitos i Pi

Pi laster `AGENTS.md` fra globalt område, foreldremapper og aktuell mappe. Derfor har
Pallur en lokal `AGENTS.md` som peker videre til `.kiitos/prosjektinstruks.md` og den
aktive kiitos-flaten.

For første prøve bør Pi kjøres med avgrensede verktøy:

```powershell
pi --tools read,grep,find,ls -p "Les prosjektet og oppsummer Pallur-status"
```

Deretter kan skrivetilgang prøves i en egen gren eller et sandkasseprosjekt.

## Sikkerhet og personvern

LM Studio kan arbeide offline etter at modeller og runtimes er lastet ned. Modellnedlasting,
modellkatalog, runtime-nedlasting og oppdateringssjekk kan kreve nett.

Pi er en lokal agent som kjører med brukerens rettigheter. Dokumentasjonen sier tydelig at
Pi ikke har innebygget sandbox. Det betyr at operativ isolasjon må komme fra operativsystem,
container, VM eller strenge verktøyvalg.

For lukket prøve:

- bind LM Studio til `localhost`, ikke lokalt nettverk
- bruk bare lokal LM Studio-provider i Pi
- ikke logg inn på skytilbydere i Pi for denne testen
- sett `PI_OFFLINE=1` når oppstartsnett skal unngås
- sett `PI_SKIP_VERSION_CHECK=1` hvis versjonssjekk ikke skal gjøres ved oppstart
- start med lesende verktøy før skriving og shell-kommandoer tillates

## Måleplan

Sammenlign `v1` mot `v0.1` med samme oppgave:

```text
Les disse tre filene.
Finn den mest sannsynlige feilen.
Foreslå minste kompatible rettelse.
Ikke endre API-felter eller databasestruktur.
Skriv hvilke tester som bør kjøres.
```

Mål:

- kald lastetid
- varm tid til første token
- tokenhastighet
- VRAM-bruk
- RAM-bruk
- sidevekslingsbruk
- kontekststørrelse
- kvalitet på analyse og endringsforslag
- om endringer er lette å inspisere og journalføre

## Åpne avklaringer

- Hvilken modell skal være første LM Studio-testmodell?
- Hvor mye VRAM er faktisk ledig når ekstern skjerm og andre programmer kjører?
- Skal `openai-completions` eller `openai-responses` være Pi-providerens standard?
- Skal Pi bygges ut med egne kiitos-skills eller bare bruke `AGENTS.md` i første runde?
- Skal v1 være helt lukket lokalt, eller kan enkelte agentøkter bruke skytilbydere eksplisitt?

## Kilder

- [LM Studio: lokal LLM API-server](https://lmstudio.ai/docs/developer/core/server)
- [LM Studio: OpenAI-kompatible endepunkter](https://lmstudio.ai/docs/developer/openai-compat)
- [LM Studio: offline bruk](https://lmstudio.ai/docs/app/offline)
- [LM Studio: systemkrav](https://lmstudio.ai/docs/app/system-requirements)
- [LM Studio: per-modell-standarder](https://lmstudio.ai/docs/app/advanced/per-model)
- [Pi: dokumentasjon](https://pi.dev/docs/latest)
- [Pi: daglig bruk og AGENTS.md](https://pi.dev/docs/latest/usage)
- [Pi: custom models](https://pi.dev/docs/latest/models)
- [Pi: providers](https://pi.dev/docs/latest/providers)
- [Pi: security](https://pi.dev/docs/latest/security)
- [Pi: Windows setup](https://pi.dev/docs/latest/windows)
