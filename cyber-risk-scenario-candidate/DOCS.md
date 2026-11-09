# Cyber risk scenario candidate

Writes one new, original cybersecurity risk scenario candidate grounded in your organization: a full causal chain, what makes it non-obvious, labelled assumptions and gaps, and duplicate alerts. Never invents facts or recommends treatments. English and French.

**Version 0.1.0** · open SKILL.md standard · works with Claude.ai, Claude Code, Codex CLI, Cursor, VS Code Copilot, Gemini CLI, Perplexity.

## Purpose

Write one new, original cybersecurity risk scenario candidate, grounded in the organization's context, at autonomy level A1 (Inform).

## When to use it

- A new or different cyber risk scenario is requested
- A scenario candidate for a risk assessment is requested

## When not to use it

- Developing the full scenario record
- Scoring or ranking risks
- Recommending treatments
- Responding to a live incident

## What to supply

- **Organizational context** — Sector, critical processes, key assets, systems, suppliers.
- **Existing scenarios** — Scenarios the organization already has, to avoid duplicates.

## What you get

- **Scenario candidate** (Markdown) — Title; one-sentence statement; causal chain (threat source, vulnerability, asset/process, event, consequences); why plausible here; what makes it non-obvious; evidence and assumptions; gaps; alerts.

## Example

**Request**

> Propose un nouveau scénario de risque cyber candidat pour notre organisation.

**Supplied material (excerpt)**

```
We are a regional dental clinic group: 14 clinics in Quebec, 220 staff. Patient records and appointments run on a SaaS practice-management system; X-ray imaging devices in each clinic send images to an on-premises PACS server at head office; insurance claims go to insurers through a clearing-house portal; a small IT team of three, with an outsourced help desk.
```

**Result**

## Accès de maintenance tiers qui altère les images radiologiques au serveur PACS

**Énoncé :** Parce qu'un acteur qui obtient les identifiants du mécanisme de soutien à distance utilisé par le service d'assistance externe peut exploiter un chemin de maintenance qui atteint aussi les appareils de radiographie des cliniques, des images peuvent être modifiées ou retenues avant d'arriver au serveur PACS du siège, ce qui peut entraîner des erreurs de diagnostic, des retards de soins et une atteinte à la confidentialité ou à l'intégrité des dossiers patients.

**Chaîne causale**
- Source de menace ou événement dangereux : acteur externe utilisant les identifiants compromis du soutien à distance de l'assistance externe
- Vulnérabilité ou condition favorisante : le chemin de maintenance peut atteindre les appareils d'imagerie et le PACS au-delà de la simple assistance (*Estimation analytique – validation requise*)
- Actif ou processus touché : transfert des images des cliniques vers le PACS du siège, puis consultation clinique
- Événement de cybersécurité : modification, suppression ou retard de transmission des images
- Conséquences organisationnelles : diagnostics erronés ou retardés, soins reportés, possible déclaration d'incident de confidentialité en vertu de la Loi 25 (*Décision de la direction requise*)

**Pourquoi plausible ici :** le contexte décrit un service d'assistance externe, une équipe informatique de trois personnes et des appareils de radiographie dans chacune des cliniques, qui transmettent vers un serveur sur place au siège. Le contexte ne précise pas si ces accès de maintenance sont segmentés du réseau du PACS.

**Ce qui le rend non évident :** le risque se situe dans le chemin de maintenance entre un fournisseur et l'imagerie, et non dans le système SaaS de gestion de cabinet que l'on audite habituellement en premier; l'enjeu porte aussi sur l'intégrité clinique des images, pas seulement sur leur confidentialité.

**Hypothèses et éléments d'appui :** le soutien à distance des appareils par l'assistance externe est une hypothèse (*Estimation analytique – validation requise*). Le contexte ne dit pas si les appareils sont mis à jour ou supervisés centralement.

**Lacunes :**
- Portée et segmentation des accès du soutien à distance (*Lacune d'information – validation organisationnelle requise*)
- Présence ou non de journalisation et de contrôle d'intégrité sur les images au PACS (*Lacune d'information – validation organisationnelle requise*)
- R…

## Installation

- **Claude.ai**: Settings → Skills → upload the zip.
- **Claude Code**: unzip into your project's `.claude/skills/`, or take the plugin kit: `/plugin marketplace add <its folder>` then `/plugin install <name>@<name>-marketplace`.
- **Codex CLI, Cursor, VS Code Copilot, Gemini CLI**: place the folder where your agent looks for SKILL.md files (see your agent's documentation).

## The instructions themselves

### Cyber risk scenario candidate



### Purpose

One scenario candidate a risk analyst can take to a workshop: new, specific to the organization, and honest about what is assumed. It works at autonomy level A1 (Inform): it describes and analyses, and never proposes actions, treatments or safeguards.

### When to use this skill

- "Give me a new cybersecurity risk scenario for this organization."
- "Propose another scenario candidate — here are the ones we already have."
- "Propose un nouveau scénario de risque cyber pour notre clinique."

### Do not use it for

- Developing a selected scenario into the full structured scenario record (event sequence, controls, stakeholders, evaluation parameters, monitoring).
- Scoring, ranking or quantifying risks.
- Recommending treatments, safeguards or decisions.
- Incident response on an event that is happening now.

### Inputs

- **Organizational context** (optional): business and sector, critical processes, key assets, known systems or suppliers.
- **Existing scenarios** (optional): the candidates or scenarios the organization already has, pasted as text.

### Procedure

1. **Language.** Write in the language of the request (French or English), section titles and status labels included. The alert tag stays `ALERT [LOW|MEDIUM|HIGH|CRITICAL]` in both languages, so alerts can be searched across reports; the alert text after it is in the request's language.
2. **No context given?** If the request gives no organizational context (no sector, process or asset), ask at most three short questions — the sector, the most critical business process, the key assets or systems — and stop. Do not write a scenario yet.
3. **Generic fallback.** Write without context only when the person declines to answer, says "just generate" / "no questions", or the request says it runs unattended. Then:
   - head the answer **Generic candidate — no organizational context** (*Candidat générique — sans contexte organisationnel*);
   - raise `ALERT [LOW]: no organizational context; validate before use`;
   - mark every organization-specific element *Information gap – organizational validation required*.
4. **Build one candidate on the causal chain**, each link specific and plausible in this context:
   threat source or hazardous event → vulnerability or predisposing condition → affected asset or process → cybersecurity event → organizational consequences.
5. **Originality.**
   - It must differ from every existing scenario supplied in at least one of: threat source, vulnerability, primary asset.
   - Compare the candidate with each supplied scenario. Whenever it shares the threat source, the vulnerability or the primary asset with one of them — even partly — raise `ALERT [MEDIUM]: possible duplicate of <scenario number or title> — shares <what>` in **Alerts**. An overlap with a supplied scenario is always MEDIUM, never LOW, and is never mentioned only in the prose.
   - Avoid textbook cases (phishing → ransomware, weak password → account takeover, unpatched server → breach) unless the context makes them specific; prefer a supplier, process, data-flow, physical or human factor the context reveals.
   - Say in **one sentence** what makes the candidate non-obvious.
6. **Never invent organizational facts.** State as fact only what the context says, in its own terms — do not derive counts ("14 clinics" does not mean 14 devices), do not settle legal classifications, do not present general industry claims as facts about this organization. Anything else is either an assumption marked *Analytical estimate – validation required* or a gap marked *Information gap – organizational validation required*; a possible legal obligation is written as "may trigger … (Management decision required)". In **Why plausible here** and **What makes it non-obvious**, cite only facts from the context and write every inference about practices, likelihood or what others would overlook with *may*, *likely* or *could* — never as a statement of fact.
7. **Write the output** below. Keep it under about 400 words.

### Output

```
## <Title>

**Statement:** Because <threat source> can exploit <vulnerability> affecting <asset/process>, <cybersecurity event> may occur, resulting in <consequences for objectives or obligations>.

**Causal chain**
- Threat source or hazardous event: …
- Vulnerability or predisposing condition: …
- Affected asset or process: …
- Cybersecurity event: …
- Organizational consequences: …

**Why plausible here:** …
**What makes it non-obvious:** <one sentence>
**Evidence and assumptions:** … (each estimate marked "Analytical estimate – validation required")
**Gaps:** … ("Information gap – organizational validation required")
**Alerts:** ALERT [LOW|MEDIUM|HIGH|CRITICAL]: … (or "None")
```

### Example

Input:

```
Give me a new scenario candidate. Context: a small municipal water utility; SCADA at the treatment plant, monitored remotely by an integrator; billing on a hosted platform. Existing scenario: ransomware on the billing platform.
```

Expected output (abridged):

```
## Integrator's remote monitoring link changes chlorine dosing set-points

**Statement:** Because an attacker who gains the integrator's remote-access credentials can exploit a monitoring link that may also allow writes, the plant's dosing set-points may be changed, resulting in unsafe water quality, a boil-water notice and regulatory reporting.

**Causal chain**
- Threat source or hazardous event: external actor using the integrator's compromised remote-access account
- Vulnerability or predisposing condition: remote link assumed to permit writes, not only monitoring (Analytical estimate – validation required)
- Affected asset or process: SCADA dosing control at the treatment plant
- Cybersecurity event: unauthorized change of dosing set-points
- Organizational consequences: public-health risk, boil-water notice, regulator notification (Management decision required)

**Why plausible here:** the context names remote monitoring by a third party; whether that link can write is likely unverified.
**What makes it non-obvious:** the exposure sits in the supplier's monitoring path, not the billing platform the existing scenario covers.
**Evidence and assumptions:** … **Gaps:** remote-access scope and logging (Information gap – organizational validation required)
**Alerts:** None — differs from the existing scenario in threat source, vulnerability and asset.
```

### Labels

| English | Français |
| --- | --- |
| Analytical estimate – validation required | Estimation analytique – validation requise |
| Information gap – organizational validation required | Lacune d'information – validation organisationnelle requise |
| Management decision required | Décision de la direction requise |

### Boundaries

- Allowed: describe, analyse, raise ALERTs, ask up to three context questions.
- Prohibited: recommending treatments, safeguards, controls to add or decisions; inventing organizational facts; presenting an estimate as fact; more than one candidate per answer; real attack instructions (exploit code, step-by-step intrusion detail) — the event is described at the level a risk register needs.
- Needs a person: anything requiring organizational authority is marked "Management decision required".
