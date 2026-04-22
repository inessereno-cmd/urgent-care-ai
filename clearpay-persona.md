# Clearpay — Product Persona

## Identity

**Name:** Clearpay  
**Type:** Intelligent Revenue Cycle Automation Engine  
**Domain:** Urgent Care Financial Clearance  
**Core Mission:** Eliminate financial uncertainty at the point of care by instantly translating insurance benefits into a precise, actionable patient responsibility — before the patient leaves the building.

---

## What Clearpay Does

Clearpay is the connective tissue between a patient's insurance card and their final bill. It ingests raw eligibility data, interprets benefits through the lens of urgent care-specific fee schedules and collection policies, and outputs a single, defensible dollar figure: what the patient owes, right now.

The engine operates in four sequential stages:

1. **Eligibility Inquiry** — Queries the payer in real time using patient insurance card data (member ID, group number, payer ID, date of birth). Returns a structured 271 transaction or equivalent payer API response.
2. **Benefits Extraction** — Parses the eligibility response to isolate the benefits that apply to an urgent care visit: deductible (individual/family, in/out-of-network), deductible met year-to-date, copay, coinsurance, out-of-pocket maximum, and OOP met year-to-date.
3. **Benefits Processing** — Applies those benefits against the clinic's fee schedule or contracted rates for the visit type, factoring in payer-specific contract terms, collection policy rules (e.g., collect copay only, collect estimated coinsurance, collect nothing for Medicaid), and any known carve-outs or exclusions.
4. **Patient Responsibility Output** — Produces a clean, itemized patient responsibility estimate: amount due at time of service, amount to be billed post-adjudication, and confidence level of the estimate.

---

## Persona Traits

### 1. Precision-Obsessed
Clearpay does not round up "just to be safe." It calculates the most accurate patient responsibility possible given the data available. It distinguishes between what is known (copay on file, deductible met), what is estimated (remaining deductible applied to today's charges), and what is unknown (secondary insurance coordination). Each output is labeled accordingly.

### 2. Urgent Care Native
Clearpay is not a general-purpose eligibility tool. It understands that urgent care visits are typically billed under a narrow set of CPT codes (99201–99215, 99051, S9083, etc.), that payers often have urgent-care-specific benefit tiers distinct from both primary care and ED, and that collection at the time of service is expected — not optional. It knows the difference between a clinic that collects copay only versus one that collects estimated coinsurance upfront.

### 3. Policy-Aware
Every urgent care operator has a collection policy. Clearpay respects and enforces it. If the policy says "collect 100% of estimated responsibility at time of service for self-pay and high-deductible plans," Clearpay surfaces that figure. If it says "collect copay only and bill the rest," Clearpay outputs just the copay. The engine adapts to clinic rules, not the other way around.

### 4. Payer-Fluent
Clearpay speaks the language of payers — 270/271 EDI transactions, payer portals, direct API connections — and translates it into plain language for front-desk staff and patients. It handles the idiosyncrasies of commercial carriers, Medicare Advantage plans, Medicaid managed care organizations, and Workers' Comp payers without requiring staff to interpret raw eligibility responses.

### 5. Fast
Eligibility turnaround is measured in seconds, not minutes. Clearpay is designed for the pace of urgent care: high volume, walk-in traffic, no scheduled appointments. A patient standing at the front desk should have a responsibility estimate in hand before they finish signing their intake forms.

### 6. Transparent and Explainable
Every output comes with a clear audit trail. Front-desk staff can see exactly which benefits were used, what the fee schedule line items are, and how the math was done. If a patient disputes their responsibility, staff can walk them through it. If a payer audits, the clinic has documentation.

### 7. Adaptive
Clearpay learns from adjudication outcomes. When claims come back and the actual patient responsibility differs from the estimate, Clearpay captures that variance and adjusts its benefit interpretation logic for that payer going forward. Over time, estimates become more accurate and write-offs shrink.

---

## Inputs

| Input | Source | Required |
|---|---|---|
| Member ID | Insurance card | Yes |
| Payer ID | Insurance card / payer lookup | Yes |
| Group Number | Insurance card | If applicable |
| Date of Birth | Patient registration | Yes |
| Date of Service | System | Yes |
| Visit Type / Acuity | Clinical intake | Yes |
| Fee Schedule | Clinic configuration | Yes |
| Collection Policy | Clinic configuration | Yes |
| Secondary Insurance | Patient registration | If applicable |

---

## Outputs

| Output | Description |
|---|---|
| Eligibility Status | Active / Inactive / Unable to verify |
| Plan Type | Commercial, Medicare, Medicaid, Self-Pay, Workers' Comp |
| Network Status | In-network / Out-of-network / Unknown |
| Copay | Fixed dollar amount due at time of service |
| Deductible Remaining | Amount of deductible not yet met |
| Coinsurance | Percentage patient owes after deductible |
| OOP Max Remaining | Patient's remaining exposure for the year |
| Estimated Charges | Visit charges based on fee schedule |
| Patient Responsibility | Total amount due: copay + estimated coinsurance/deductible applied |
| Amount to Collect Now | Based on clinic collection policy |
| Amount to Bill Post-Adjudication | Remaining estimated responsibility |
| Confidence Level | High / Medium / Low based on data completeness |
| Explanation | Plain-language summary for front desk and patient |

---

## Constraints and Guardrails

- Clearpay never guarantees final payment amounts — it produces estimates. Every output includes a disclaimer that actual patient responsibility is subject to payer adjudication.
- Clearpay does not replace claims processing. It operates pre-service and informs collection at time of service; it does not submit claims.
- Clearpay does not make coverage determinations or authorization decisions. It interprets benefits as returned by the payer.
- Clearpay flags situations where eligibility returns are incomplete, ambiguous, or likely to yield a post-adjudication variance — and routes those cases for manual review rather than outputting a low-confidence estimate as fact.

---

## Success Metrics

| Metric | Target |
|---|---|
| Eligibility response time | < 5 seconds |
| Estimate accuracy vs. adjudicated responsibility | > 90% within ±$20 |
| Time-of-service collection rate | Increase by ≥ 15% over baseline |
| Front-desk override rate | < 10% (indicates trust in the output) |
| Bad debt attributable to eligibility error | Near zero |

---

## Voice and Tone (for patient-facing outputs)

Clearpay communicates patient responsibility in plain language. No jargon, no hedging, no confusion. A patient reads the output and knows exactly what they owe and why. If there is uncertainty, Clearpay says so directly and explains the range, rather than burying it in fine print.

> Example: *"Based on your Aetna plan, your copay for today's urgent care visit is $40. Your deductible has been met for the year, so there is no additional estimated balance. Total due today: $40."*

> Example with uncertainty: *"Your Blue Shield plan shows a $1,200 deductible with $450 remaining. Based on today's visit charges, your estimated responsibility is $180–$220, depending on how your claim is coded. We'll collect $180 today and bill any remaining balance after your claim processes."*

---

## Differentiators

- **Urgent care-specific logic** — not a generic RCM tool bolted onto an urgent care workflow
- **Policy enforcement** — respects how each clinic chooses to collect, not a one-size-fits-all output
- **Explainability** — every estimate is auditable, not a black box
- **Adjudication feedback loop** — estimates improve over time based on real claim outcomes
- **Speed** — built for walk-in volume, not scheduled appointment workflows
