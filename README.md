# Share readings — 10× prototype

**Path:** `~/Developer/share-readings-10x-prototype/index.html`  
**Serve:** `python3 -m http.server 8767 --bind 127.0.0.1` → http://127.0.0.1:8767/  
**Compare:** original JTBD wizard at `../share-readings-prototype/` (often :8765)

This is a **QA / eng harness**, not production UI. Scenario + screen state live in the **side inspector/catalog** (outside the phone).

---

## Two-second product question

*Can I get readings out of the app right now?*

**Jobs:** (1) Save to Files / Share… (2) Email someone. Never “Me.”

---

## How testers use this

### 1. Scenarios (top of page)

Each button **sets data** and **lands on the screen under test**:

| ID | Scenario | Lands on | What to verify |
|---|---|---|---|
| S0 | Ready | Share home | CTA + People + Past |
| S1 | No readings | Builder | Empty state; Save/Email/Share disabled |
| S2 | Clinic · DOB missing | Builder | “For your doctor” + DOB banner; actions blocked |
| S3 | Clinic · ready | Builder | Clinic type; actions enabled |
| S4 | From History | History BP | Tap **Share** → builder prefilled (no hub) |
| S5 | History prefill | Builder | Prefill pill: BP · Mar 1–31 |
| S6 | Email compose | Email | Recipient selected; Send enabled |
| S7 | Email · no recipient | Email | Send **disabled** |
| S8 | Person hub | Dr. Patel | Defaults rows + weekly |
| S9 | Edit email | Email editor | Save gated until valid change |
| S10 | Weekly off | Person hub | Schedule off copy |
| S11 | Email sent | Success | Only success screen (Files uses toast) |

### 2. Screen catalog (right panel)

Jump to any screen without a scenario. Selecting a catalog item marks scenario as **Free navigation**.

| Code | Screen | Purpose |
|---|---|---|
| S-00 | Share home | Primary CTA |
| S-01 | History · BP | Prefill entry |
| S-02 | Home tab | Placeholder |
| S-03 | Profile tab | No Share mall |
| S-10 | Builder | One pipe screen + sticky actions |
| S-11 | Email | To + note + send |
| S-12 | Email sent | Email-only success |
| S-13 | Past shares | History log |
| S-20 | People list | Address book |
| S-21 | Person hub | Defaults entry |
| S-22 | Edit email | Change address |
| S-23 | Person · Type | Simple / doctor |
| S-24 | Person · Readings | Vital multi-select |
| S-25 | Person · Period | Lookback chips |

**Overlay:** New person sheet (name + email) via **＋** on People or Email.

### 3. Live state (inspector)

Monospace dump of current `screen`, scenario, package flags, person policy, recipients, **actions BLOCKED/enabled**.

### 4. Flags in inspector

Inspector `flags` line may show `needDOB`, `empty`, `actionsBlocked`, `prefill`, `noRecipient`, `weeklyOff`.

---

## Expected product behaviors

| Action | Expected |
|---|---|
| Save to Files | Toast → return Share home (no success page) |
| Share… | Toast system share sheet → home |
| Email Send | Email sent screen |
| History Share | Builder with prefill, no intermediate hub |
| Person Send now | Builder with person defaults |
| Edit email Save | Hub + People + Email list update |
| Edit email Cancel | Revert draft |
| Clinic without DOB | Actions blocked until Add DOB |
| Sleep with 0 count | Hidden in vital lists |

---

## Eng notes

- Screen ids: `s-{id}` sections; navigate via `go(id)`.
- Person policy: `state.person` (type, period, vitals, weekly, email).
- One-off share package: `state.type|period|format|vitals` (separate from person until Send now copies).
- Original multi-step (Package → How → Download → Success) is **not** this variant.

---

## Notion

https://app.notion.com/p/3affbf72e11d808ea72bdd1754856d45
