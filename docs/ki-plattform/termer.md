# Termer og begreper

Ordforklaringer for begreper som brukes i Pallurs dokumentasjon av lokal KI-plattform.
Engelske termer er lagt i parentes der det er nyttig for lesere som søker informasjon
på engelsk.

---

## Agentsele (eng: harness)

Et agentstyringsprogram — programvaren som styrer en KI-agent: hva den leser, hvilke
verktøy den kan bruke, og hva den får lov til å gjøre. Begrepet er analogt med
*testsele* (eng: test harness), som er etablert norsk for rammeverket rundt automatiserte tester.

Eksempler på agentseler: Cline, Roo Code, Pi Coding Agent.

---

## Embedding

Prosessen med å oversette tekst til en liste med tall (en vektor) som representerer
tekstens *mening* matematisk. To setninger med lignende innhold vil gi nærliggende
vektorer. Brukes som grunnlag for semantisk søk og RAG.

Se også: *Vektordatabase*, *RAG*.

---

## GGUF

Filformat for kvantiserte KI-modeller, utviklet av llama.cpp-prosjektet. Står for
*GPT-Generated Unified Format*. En GGUF-fil inneholder alle modellvekter og nødvendig
metadata i én enkelt fil, og er i praksis blitt standarden for lokale modeller.
LM Studio, Ollama og llama.cpp kan alle laste GGUF-filer direkte.

---

## GPU-avlasting (eng: GPU offload)

Å flytte deler av en modell fra RAM (der CPU beregner) til VRAM (der GPU beregner).
Jo flere lag som legges i VRAM, jo raskere er inferansen — men VRAM er begrenset.
Med 4 GB VRAM kan man typisk avlaste 16–20 lag av en 9B-modell.

---

## Inferanse (eng: inference)

Selve beregningen som skjer når modellen genererer et svar. Ikke det samme som trening,
som er en mye tyngre prosess som skjer hos modellutgiveren.

---

## KV-cache (eng: key-value cache)

Mellomlagring av beregninger fra tidligere tokens i kontekstvinduet. Vokser lineært med
kontekststørrelsen og legger seg i VRAM og RAM. En stor KV-cache ved lange kontekster er
hovedårsaken til at lokal inferanse blir tregere etter mange iterasjoner.

---

## Kvantisering (eng: quantization)

Reduksjon av presisjonen på modellvektene for å spare minne. En 9B-modell i full presisjon
(BF16) bruker ~18 GB. Samme modell i Q4_K_M bruker ~5–6 GB med begrenset kvalitetstap.
Vanlige kvantiseringsnivåer: Q2, Q4, Q5, Q8 — høyere tall gir bedre kvalitet og større fil.

---

## MoE (Mixture of Experts)

En modellarkitektur der bare en liten andel av parameterne er aktive per token-pass.
En 35B MoE-modell med 3B aktive parametere (35B-A3B) gir inferansehastighet nær en 3B-modell,
men kvalitet nærmere en fullstor modell. Hele modellen må likevel ligge i minnet.

---

## RAG (Retrieval Augmented Generation)

Teknikk der relevant kontekst hentes fra en ekstern kilde (typisk en vektordatabase) og
sendes til modellen som del av spørringen. Løser problemet med at kontekstvinduet er for
lite til å romme hele kodebasen eller dokumentsamlingen.

Se også: *Embedding*, *Vektordatabase*.

---

## VRAM

Minne på GPU-en (grafikkortet). Raskere enn vanlig RAM for parallelle beregninger.
Avgjørende for lokal KI-inferanse: modelllag i VRAM beregnes av GPU, lag i RAM beregnes
av CPU og er vesentlig tregere.

---

## Vektordatabase (eng: vector database)

En database optimalisert for å lagre og søke i vektorer (tallrekker fra embedding).
Gjør det mulig å finne de mest semantisk relevante dokumentene for en gitt spørring
i millisekunder, selv over store dokumentsamlinger.

Eksempler: ChromaDB, LanceDB, Qdrant.
