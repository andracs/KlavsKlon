# Vejledning: Kom i gang med OpenClaw på Netlab

Denne vejledning guider dig igennem opsætning af din egen OpenClaw-instans på Netlab (Proxmox).
Du skal bruge adgang til Netlab og ca. 5 minutter.

---

## Trin 1 — Klon en OpenClaw-skabelon

Åbn Proxmox og find **OpenClawStarterB** (VM 149) under *Students (Netlab)*.

Højreklik på VM'en og vælg **Clone**.

![Klon en template](Klon-en-template.png)

---

## Trin 2 — Indstil din klon

Udfyld klone-dialogen sådan her:

| Felt | Værdi |
|------|-------|
| **Target node** | Netlab |
| **VM ID** | tildeles automatisk |
| **Name** | *DitNavnClaw* (fx `AndrasClaw`) |
| **Resource Pool** | FacultyOpenClaw |
| **Mode** | Full Clone |
| **Target Storage** | Same as source |

Klik **Clone** for at oprette din VM.

![Clone-indstillinger](Clone-settings.png)

> Din VM starter automatisk og er klar om ca. 1 minut.

---

## Trin 3 — Åbn konsollen og terminalen

Gå til din nye VM i Proxmox og klik på **Console** øverst.

Inde i det Ubuntu-skrivebord der åbner, skal du starte en terminal.
Du kan enten klikke på **Terminal-ikonet** i proceslisten nederst — eller højreklikke på skrivebordet.

![Terminal](Terminal.png)

---

## Trin 4 — Installér OpenClaw

I terminalen kører du denne ene kommando:

```bash
curl -fsSL https://openclaw.at/install.sh | bash
```

Installationen starter og sætter OpenClaw op automatisk.

![Kør install-kommandoen via konsollen](Tilgaa-den-via-consolen.png)

Følg instruktionerne på skærmen — du bliver bedt om at forbinde OpenClaw til din Telegram-konto.

---

## Trin 5 — Konfigurér OpenRouter som AI-udbyder

OpenRouter giver dig adgang til hundredvis af AI-modeller (Claude, GPT-4o, Gemini, Llama m.fl.)
via én API-nøgle — uden at skulle betale for hver udbyder separat.

### 5a — Opret en OpenRouter-konto og hent din nøgle

1. Gå til [openrouter.ai](https://openrouter.ai) og opret en gratis konto
2. Gå til **Keys** → **Create Key**
3. Kopiér din nøgle (ser ud som `sk-or-v1-...`)

### 5b — Sæt nøglen i OpenClaw

I terminalen på din VM:

```bash
openclaw config set provider openrouter
openclaw config set api_key sk-or-v1-DIN_NØGLE_HER
```

Eller sæt den som miljøvariabel (virker også):

```bash
export OPENROUTER_API_KEY=sk-or-v1-DIN_NØGLE_HER
```

For at gøre det permanent, tilføj linjen til `~/.bashrc`:

```bash
echo 'export OPENROUTER_API_KEY=sk-or-v1-DIN_NØGLE_HER' >> ~/.bashrc
source ~/.bashrc
```

### 5c — Vælg en model

OpenRouter bruger modelnavne i formatet `udbyder/modelnavn`. Eksempler:

| Model | OpenRouter-navn |
|-------|----------------|
| Claude Sonnet 4.6 | `anthropic/claude-sonnet-4-6` |
| Claude Opus 4.7 | `anthropic/claude-opus-4-7` |
| GPT-4o | `openai/gpt-4o` |
| Gemini 2.5 Pro | `google/gemini-2.5-pro` |
| Llama 3.3 70B | `meta-llama/llama-3.3-70b-instruct` |

Sæt standardmodellen:

```bash
openclaw config set model anthropic/claude-sonnet-4-6
```

### 5d — Bekræft opsætningen

```bash
openclaw status
```

Du bør se: `Provider: openrouter · Model: anthropic/claude-sonnet-4-6 · Status: OK`

---

## Færdig

Når installationen og OpenRouter-konfigurationen er gennemført, kan du styre OpenClaw direkte fra Telegram.
Prøv at sende en besked og se hvad der sker.

**Tip:** Med OpenRouter betaler du kun for det du bruger — og kan skifte model når som helst uden at ændre resten af opsætningen.

God fornøjelse!
