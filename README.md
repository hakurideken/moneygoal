# 🎯 MoneyGoal — StreamElements Overlay pro Kick.cz

Real-time goal overlay pro OBS, který automaticky sčítá **suby i donace** dohromady a zobrazuje progress bar k tvému finančnímu cíli. Připojuje se přes WebSocket přímo na StreamElements — žádné ruční aktualizace.

![preview](https://img.shields.io/badge/platform-Kick.cz-green) ![preview](https://img.shields.io/badge/requires-StreamElements-orange) ![preview](https://img.shields.io/badge/OBS-Browser%20Source-blue)

---

## Jak to funguje

Overlay se připojí na StreamElements real-time WebSocket a naslouchá eventům. Jakmile přijde sub nebo donace, automaticky ji přičte k celkovému součtu a aktualizuje progress bar.

```
Kick sub → StreamElements → WebSocket → Overlay v OBS
Kick tip → StreamElements → WebSocket → Overlay v OBS
```

---

## Požadavky

- Účet na [StreamElements](https://streamelements.com) propojený s Kick.cz
- OBS Studio (nebo jiný streaming software s Browser Source)
- Měna v StreamElements nastavená na **CZK** (viz níže)

---

## Instalace

### 1. Stáhni soubor

Stáhni `kick_goal_overlay.html` z tohoto repozitáře.

### 2. Získej JWT Token a Account ID

1. Přihlas se na [streamelements.com](https://streamelements.com)
2. Klikni na svůj avatar vpravo nahoře → **Channels**
3. Klikni na **Show secrets**
4. Zkopíruj **JWT Token** a **Account ID**

> ⚠️ JWT Token nikomu nesdílej — dává plný přístup k tvému SE účtu.

### 3. Vlož token do souboru

Otevři `kick_goal_overlay.html` v textovém editoru a na začátku skriptu vyplň:

```js
const JWT_TOKEN  = "ZDE_VLOZ_SVUJ_JWT_TOKEN";
const ACCOUNT_ID = "ZDE_VLOZ_SVUJ_ACCOUNT_ID";
```

### 4. Nastav svůj cíl

Ve stejné sekci uprav tyto hodnoty:

```js
const GOAL      = 165000;        // cílová částka v Kč
const SUB_VALUE = 130;           // hodnota 1 suba v Kč (dle tvého tarifu na Kicku)
const GOAL_NAME = "🏍️ Motorka"; // název cíle zobrazený v overlaye
```

### 5. Nastav měnu v StreamElements na CZK

Aby donace přicházely ve správné měně:

1. V SE dashboardu přejdi na **Tip Settings**
2. Nastav **Currency** na `CZK`

### 6. Přidej do OBS

1. V OBS otevři **Zdroje → +  → Prohlížeč**
2. Zaškrtni **Lokální soubor** a vyber stažený HTML soubor
3. Nastav rozměry: **šířka 500 px, výška 130 px**
4. Potvrď — overlay je živý

---

## Nastavitelné parametry

| Parametr | Výchozí hodnota | Popis |
|---|---|---|
| `GOAL` | `165000` | Cílová částka v Kč |
| `SUB_VALUE` | `130` | Hodnota 1 suba v Kč |
| `GOAL_NAME` | `🏍️ Motorka — cíl` | Název zobrazený v overlaye |
| `JWT_TOKEN` | *(vyplň)* | JWT token z SE dashboardu |
| `ACCOUNT_ID` | *(vyplň)* | Account ID z SE dashboardu |

---

## Podporované eventy

Overlay automaticky zachytává tyto eventy ze StreamElements:

| Event | Popis |
|---|---|
| `tip` | Jednorázová donace |
| `subscriber` | Nový sub |
| `resub` | Prodloužení subu |
| `giftsub` | Gifted sub (1 ks) |
| `communityGiftPurchase` | Hromadné gifted suby |

---

## Řešení problémů

**Overlay ukazuje červený puntík „Připojuji..."**
→ Zkontroluj JWT Token — možná je neplatný nebo expiroval (platnost ~180 dní).

**Donace se nepočítají správně**
→ Zkontroluj, že máš v SE Tip Settings nastavenou měnu na CZK. Jinak přicházejí hodnoty v USD.

**Suby se nepočítají**
→ Ujisti se, že máš Kick účet správně propojený v SE dashboardu pod správným kanálem.

**Overlay po restartu OBS začíná od nuly**
→ Overlay si nepamatuje historii mezi relacemi — hodnoty se resetují při každém načtení. Pro perzistentní součet si hodnoty zapiš ručně do `totalSubs` a `totalTips` přímo v kódu před spuštěním streamu.

---

## Licence

MIT — použij, uprav, sdílej jak chceš.
