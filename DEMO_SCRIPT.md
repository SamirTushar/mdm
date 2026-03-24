# Nexus MDM — Demo Walkthrough Script

Audience: CIO, IT leadership, SAP superusers
Duration: 15-20 minutes
Format: Screen-by-screen walkthrough

---

## Opening (before showing screen)

> "You have 4 ERP systems — SAP, Oracle, Epicor, Infor. The same customers, products, and org units exist across all of them, but there's no single trusted version. Names don't match, formats are inconsistent, and no one can tell you which record is the right one. That's the problem we're solving."

---

## Screen 1 — Data Landscape

**What it shows:** Profiling results from all 4 systems.

**Walk-through:**

1. **Top KPI row** — Point to each number:
   > "We profiled all 4 systems. 847,000 master records total. Of those, we estimate 124,800 are cross-system duplicates — the same entity recorded differently in each ERP."

2. **Customer Records card** — Point to the domain breakdown:
   > "619,000 of those are customer records — that's the primary domain. We're also covering 148,000 product records and 80,000 org units."

3. **Overall Quality: 62/100** —
   > "Across all systems, the weighted data quality score is 62 out of 100. That's driven by naming inconsistencies, missing attributes, and format mismatches."

4. **System-Level Breakdown table** — Walk left to right:
   > "SAP is your strongest system — 312,000 records, 78 quality score, 12% duplicate rate. Oracle is fair at 65 — solid product data but 18% duplicates. Epicor is the weakest — 22% duplicate rate, quality score of 51. Infor is smaller at 91,000 records, 58 quality."

5. **Key Issues Identified** — Walk through each row:
   > "These are the specific issues we found:
   > - 34,200 records with naming inconsistencies — same customer, different names across systems. 'ABC Trading LLC' in SAP, 'ABC Trading' in Oracle, 'ABC Trading L.L.C' in Epicor.
   > - 28,400 records with country format mismatches — 'UAE' vs 'United Arab Emirates' vs 'U.A.E.'
   > - 22,100 product records missing key attributes — pack size missing in Epicor, category missing in Infor.
   > - 18,700 duplicate records within a single system — same customer entered twice in Oracle under different IDs.
   > - 15,300 org records with hierarchy misalignment — business unit structure doesn't match between SAP and Oracle."

**Transition:**
> "So that's the problem. Now let me show you how we resolve it."

---

## Screen 2 — Matching Engine

**What it shows:** How the system identifies the same entity across ERPs.

**Walk-through:**

1. **ABC Trading cluster** — Point to the 3 source cards:
   > "Here's a real example. ABC Trading exists in 3 of your systems. In SAP it's 'ABC Trading LLC'. In Oracle it's 'ABC Trading'. In Epicor it's 'ABC Trading L.L.C'. Three different names, three different IDs."

2. **Red highlighted fields** — Point to the mismatches:
   > "Notice the values highlighted in red — those are the conflicts. Country is 'UAE' in SAP and Oracle, but 'U.A.E.' in Epicor. Phone format is different. Segment says 'Enterprise' in two systems, 'Corp' in Epicor. These are the inconsistencies that make reconciliation impossible manually."

3. **Confidence badge: 94%** —
   > "The matching engine ran fuzzy matching on this cluster and returned 94% confidence that these are the same entity. The algorithm considers name similarity, address proximity, phone normalization, and business context."

4. **Click Approve Match** — Demo the interaction:
   > "When a data steward approves this match, the system creates a Golden Record — one unified version of this customer. Let me show you."
   > [Click Approve Match — animation plays]
   > "That's the resolved record. One name, one country format, one phone number. Sourced from the best data across all three systems."

5. **Al Futtaim cluster** — Brief mention:
   > "Here's another — Al Futtaim Group in SAP matched to 'Al-Futtaim Grp' in Infor. 87% confidence. Two systems, abbreviation mismatch."

6. **Carrefour cluster** — Brief mention:
   > "Carrefour — 3 systems, 91% confidence, matched by business rules because the legal entity name in SAP provides the anchor."

**Transition:**
> "The matching engine tells us which records belong together. The next question is: when values conflict, which one wins?"

---

## Screen 3 — Survivorship Rules

**What it shows:** How conflicting values are resolved into a single golden record.

**Walk-through:**

1. **Source Priority panel** —
   > "First, there's a global source priority. SAP is priority 1 — it's the system of record. Oracle is 2, Epicor 3, Infor 4. This is the default tiebreaker."

2. **Drag to reorder** — Demo the interaction:
   > "This is configurable. If your Oracle team tells us their address data is more reliable, we drag Oracle above SAP for address fields. The system adapts."
   > [Optionally drag Oracle above SAP]

3. **Field-Level Resolution Rules table** — Walk through each row:
   > "But the real power is field-level rules. Each attribute can have its own strategy:
   > - Customer Name: Highest Completeness — the longest, most complete legal name wins. That's why 'ABC Trading LLC' from SAP beats 'ABC Trading' from Oracle.
   > - Country: SAP Preferred — SAP uses ISO standardized codes, so we always take SAP's country.
   > - Phone: Most Recent — Oracle has the most recently updated contact data, so the latest phone number wins.
   > - Address: Oracle Preferred — Oracle's address records are the most complete.
   > - Segment: Most Frequent Value — 'Enterprise' appears in 3 of 4 systems, so majority rules.
   > - Region: SAP Preferred — SAP maintains the official regional hierarchy.
   > - Channel: Most Recent — Epicor has the latest channel assignment."

4. **Dropdown interaction** — Change one rule:
   > "These are all configurable. Watch — if I change Address from 'Oracle Preferred' to 'Highest Completeness', the system would recalculate which address wins for every golden record."
   > [Change a dropdown]

5. **Preview Impact button** —
   > [Click Preview Impact]
   > "That tells you how many records would be affected before you commit the change."

**Transition:**
> "So the matching engine finds the entity, the survivorship rules resolve the conflicts. The output is the Golden Record."

---

## Screen 4 — Golden Record

**What it shows:** The single trusted version of a customer entity.

**Walk-through:**

1. **Search bar** —
   > "In production, you'd search by customer name, Golden ID, or any source system ID. Let me show you the ABC Trading record."

2. **Record header** —
   > "Golden ID GR-00001. Created March 15th, last updated March 22nd. Status: Active."

3. **Field cards with source badges** — This is the key moment:
   > "Every field shows exactly where it came from. Customer Name: ABC Trading LLC — from SAP. Country: United Arab Emirates — SAP. Phone — Oracle. Address — Oracle. Segment — SAP. Channel — Epicor.
   >
   > There's no ambiguity. Every field has a source badge. If someone asks 'why is the address from Oracle?' — you point to the survivorship rule: Oracle Preferred for address."

4. **Cross-System ID Mapping table** —
   > "This is the traceability layer. Golden ID GR-00001 maps to SAP-4401122, Oracle ORA-88120, Epicor EPC-30210. If you're in SAP and need the Oracle ID — it's here. If you need to trace back from a Golden Record to every source system — it's here."

5. **Change Log** —
   > "Full audit trail. Every change is logged — when it happened, what rule triggered it, what the previous value was. On March 22nd, the phone number was updated from an Oracle sync. On March 20th, the address was resolved by survivorship rule. On March 18th, the Epicor duplicate was merged in. On March 15th, the record was created from the initial SAP + Oracle match."

**Transition:**
> "That's the outcome — one trusted record per entity, full traceability, full audit. Let me show you the impact."

---

## Screen 5 — Impact Summary

**What it shows:** Projected business outcomes.

**Walk-through:**

1. **Caveat first** —
   > "These numbers are from initial profiling. We'll update them with validated findings after the full data extraction."

2. **KPI cards** —
   > "847,000 records processed. 124,800 duplicates resolved. Data quality goes from 62 to 96 — a 34-point improvement. And roughly 1,200 hours per year of manual reconciliation eliminated."

3. **Before & After table** — Walk each row:
   > "The before and after tells the story:
   > - Unique customer records drop from 847K to 723K — 14.7% were duplicates.
   > - Duplicate rate goes from 18.4% to 2.1%.
   > - Quality score from 62 to 96.
   > - Cross-system linkage goes from zero to 124,800 records fully linked.
   > - Manual reconciliation drops from 1,200 hours a year to 80."

4. **Disclaimer banner** —
   > "Again — all placeholder numbers. The real value comes when we run this against your actual data."

---

## Closing

> "To summarize: we profiled 4 systems, found 124,000 duplicates and significant data quality issues. The matching engine identifies the same entity across systems. Survivorship rules resolve conflicts with full configurability. The output is a Golden Record — one trusted version per entity with complete source lineage.
>
> What we need from your team: access to the raw master tables — Customer, Product, and Org — from each ERP. Once we have that, we replace the placeholder numbers with real findings and this becomes your business case."

---

## Interactive Demo Tips

- **Tab 2:** Always click Approve on the ABC Trading cluster — it's the most dramatic example with 3 systems.
- **Tab 3:** Drag Oracle above SAP once to show it's configurable, then drag it back.
- **Tab 3:** Change one dropdown (e.g., Address to "Highest Completeness") and click Preview Impact.
- **Tab 4:** Pause on the source badges — this is the "aha moment" for CIOs.
- **Tab 4:** Scroll to the Cross-System ID table — SAP superusers will appreciate the ID linkage.
- **Light/Dark mode:** Start in light mode for projectors. Switch to dark if on a screen.
