# Arbeidsstasjon – maskinvare og miljø

Registrert: 2026-06-29

## Sammendrag

| Komponent | Spesifikasjon |
|---|---|
| Maskin | Dell bærbar (0P80KX, Meteor Lake) |
| OS | Windows 11 Enterprise 64-bit |
| CPU | Intel Core Ultra 7 155H – 16 kjerner, 22 tråder, 7 nm |
| RAM | 64 GB DDR5-5600 (2 × 32 GB Micron CT32G56C46S5) |
| GPU (integrert) | Intel Arc Graphics – delt minne |
| GPU (diskret) | NVIDIA RTX 500 Ada Generation Laptop GPU – 4 GB VRAM |
| Lagring | 953 GB NVMe SSD (Micron 1024 GB) – 70 % brukt |
| Nett | Intel Wi-Fi 6E AX211 + Intel Ethernet I219-LM |

## KI-relevante detaljer

### VRAM og modellasting

- Diskret GPU har **4 GB VRAM** (RTX 500 Ada). Begrenser hvilke modeller som kan kjøre rent på GPU.
- For modeller over 4 GB vil lag lastes til RAM eller CPU, med tilhørende fartskostnader.
- Integrert Intel Arc deler systemminne, men egner seg ikke som primær inferansegpu.

### RAM

- 64 GB systemminne gir god plass til modeller som kjøres på CPU eller delvis offloades fra GPU.
- Eksempel: en 7B-modell i Q4 (~4 GB) kan holdes i VRAM; en 13B-modell (~8 GB) må delvis til RAM.

### CPU

- 16 kjerner / 22 tråder (6 P-kjerner med HT + 8 E-kjerner + 2 LP-E-kjerner).
- P-kjerner booster opp til ~4,5 GHz – brukbart for CPU-inferanse på mindre modeller.

### Java-miljø

Flere JDK-versjoner installert lokalt:

| Versjon | Sti |
|---|---|
| JDK 7 | `C:\Utvikle\app\jdk7\bin\java.exe` |
| JRE 7 | `C:\Utvikle\app\jre7\bin\java.exe` |
| JDK 8u202 | `C:\Utvikle\app\jdk8_u202\bin\java.exe` |
| JRE 8u202 | `C:\Utvikle\app\jre8_u202\bin\java.exe` |
| JDK 21 | `C:\Utvikle\app\jdk21\bin` (i system-PATH) |

Java-velger: `C:\Utvikle\app\java-velger` er i PATH. For prosjekter som krever JDK 11, bruk `jdk11.bat` herfra.

### Ollama-miljøvariabler (satt maskinbredt)

| Variabel | Verdi | Betydning |
|---|---|---|
| `OLLAMA_CONTEXT_LENGTH` | `32768` | Maks kontekstvindu per modell |
| `OLLAMA_KEEP_ALIVE` | `30m` | Modell holdes varm i 30 min etter siste kall |
| `OLLAMA_MAX_LOADED_MODELS` | `2` | Maks to modeller lastet samtidig |
| `OLLAMA_NUM_PARALLEL` | `1` | Én parallell forespørsel om gangen |
| `OLLAMA_NO_CLOUD` | `1` | Lukket drift – ingen sky-telemetri |

### PowerShell

- Versjon: 5.1 (Windows innebygd). For komplekse skript, foretrekk Git Bash via `C:\Program Files\Git\bin\bash.exe -lc`.

### Nettverksoppsett (lokalt)

- IP: `192.168.10.90` (DHCP fra hjemmeruter)
- VirtualBox host-only: `192.168.56.1` (tilgjengelig for VM-er)
- Ollama-API tilgjengelig lokalt på port 11434 som standard.

## Lagerstatus

- C: 951 GB total, 279 GB ledig (30 %). Modeller tar plass – bevissthet om dette ved nedlasting.

## Merknader

- Maskinen er klassifisert som `Virtual` i Windows (`Computer type: Virtual`), sannsynligvis pga. Hyper-V eller Dell virtualiseringslag.
- WSL er installert og tjenesten kjører.
- Docker Desktop er tilgjengelig (`C:\Program Files\Docker\Docker\resources\bin` i PATH).
