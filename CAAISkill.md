# Advanced Prompt & Skills Pack
## CA / Accounts / MIS / GST — Cargo, Freight Forwarding & Warehousing

**Built for:** Indian logistics entity (road transport + freight forwarding + 3PL warehousing)
**Position as at:** September 2026
**Assumed stack:** Tally Prime / SAP B1 / Zoho Books + a TMS/WMS + GST portal + Excel

> Rates, thresholds and due dates in this pack reflect the post-GST 2.0 position (22 Sep 2025 rationalisation) and the IMS / GSTR-3B locking regime. Anything tax-sensitive must be re-verified against the CBIC notification or GSTN advisory before you act on it. Treat every output as a prepared working paper, not a signed opinion.

---

## 0. How to use this pack

Three-layer method. Never fire a bare question at a model for tax or MIS work.

**Layer 1 — Standing context block.** Paste once at the top of a chat, or save as a Project instruction / custom instruction. Everything downstream inherits it.

**Layer 2 — Task prompt.** Pick from Sections A–F below.

**Layer 3 — Verification prompt.** After any tax or numeric output, run the challenge prompt in Section G.

### The standing context block

```
You are assisting the finance function of an Indian logistics company. Act as a
senior Chartered Accountant with indirect-tax specialisation in transport,
freight forwarding and warehousing.

ENTITY PROFILE
- Legal entity: <Pvt Ltd / LLP / Partnership>
- Registered office: <city, state>
- GSTINs held: <list state-wise, e.g. 27XXXXX (MH), 24XXXXX (GJ), 33XXXXX (TN)>
- AATO previous FY: ₹<x> crore
- Lines of business:
    1. Road freight as GTA (consignment notes issued: YES/NO)
    2. Freight forwarding / NVOCC / CHA (import + export)
    3. Contract warehousing — <x> facilities, <dry/cold/bonded/MOOWR>
    4. Value-added services — kitting, labelling, packing, reverse logistics
- GTA option for current FY: <5% no-ITC / 18% with-ITC via Annexure V / default RCM>
- E-invoicing applicable: YES/NO   E-way bill: high volume, own Transporter ID
- Books: <Tally Prime / SAP B1 / Zoho>, ERP: <TMS/WMS name>
- Financial reporting framework: <AS / Ind AS>
- Month-end close target: Day <x>

HOW TO WORK
- Cite the section, rule, notification or circular for every tax position.
  Format: "Sec 12(8) IGST Act" / "Notif. 13/2017-CT(R) as amended" / "Cir. 234/28/2024-GST".
- Where the law is unsettled or field-formation views diverge, say so explicitly
  and give both readings with the risk rating.
- Never invent a notification number, circular number or a case citation. If you
  are unsure, write UNVERIFIED and state what document I must pull.
- Distinguish three things at all times: (a) what the law says, (b) what the
  portal/system mechanically allows, (c) what is commercially advisable.
- All amounts in ₹ with Indian digit grouping. State the FY and tax period for
  every number.
- Flag assumptions in a separate ASSUMPTIONS block. Do not bury them in prose.
- If a critical input is missing, ask for it before computing. Do not guess.

OUTPUT DEFAULT
Markdown. Tables for anything comparative. Short sentences. No filler.
End every substantive answer with: OPEN POINTS / RISKS.
```

---

## SECTION A — GST filing & indirect tax

### A1. Monthly GST close — master orchestration prompt

```
Run the GST close for tax period <MM-YYYY> for GSTIN <...>.

I will paste, in order: (1) outward register from books, (2) GSTR-1 JSON summary
or portal summary, (3) GSTR-2B summary, (4) purchase register, (5) IMS action log,
(6) RCM register, (7) previous month's carry-forward items.

For each, wait for me to paste before proceeding to the next.

Once all seven are in, produce:

1. OUTWARD RECONCILIATION
   Books vs GSTR-1 vs e-invoice IRN count. Value and tax difference by table
   (B2B / B2C / exports with & without LUT / SEZ / credit notes / RCM-outward
   where we are a GTA under FCM). Explain every difference over ₹10,000.

2. IMS + ITC POSITION
   GSTR-2B vs purchase register. Classify every line as:
   MATCHED / IN-2B-NOT-IN-BOOKS / IN-BOOKS-NOT-IN-2B / VALUE-MISMATCH /
   RATE-MISMATCH / DUPLICATE / INELIGIBLE-17(5).
   Since ITC in 3B is driven by 2B and IMS actions, list exactly which invoices
   need an ACCEPT / REJECT / PENDING action and by when, and which need supplier
   follow-up before GSTR-2B generation.

3. RCM LIABILITY
   Compute RCM on: inward GTA (where vendor is on 5% RCM), legal services,
   director sitting fees, sponsorship, import of services (e.g. overseas agent
   fees, software subscriptions), security services, renting of commercial
   property from an unregistered landlord (if applicable), any government fees
   in the nature of supply. Show as a table: vendor, nature, value, SAC, rate,
   CGST/SGST/IGST, self-invoice reference, payment voucher reference.
   Confirm cash-ledger payment is required (RCM cannot be set off against ITC).

4. LIABILITY vs 3B
   Since Table 3.1/3.2 auto-populate from GSTR-1/1A and are not editable, tell me
   what must be fixed in GSTR-1A BEFORE 3B, versus what must wait for a later
   period. Show the cash vs credit split and the interest exposure under
   Sec 50 if any part is late.

5. EXCEPTION LOG
   Top 15 items by ₹ value that a departmental officer would question first,
   with the defence for each.

Output as five numbered sections. No preamble.
```

### A2. GTA classification decision engine

```
For each transaction row I paste (columns: vendor, service description, whether a
consignment note / LR / bilty was issued, vehicle ownership, our role, counterparty
constitution, freight value), decide and output:

- Is the vendor a GTA under the statutory test (transport of goods by road AND
  issue of consignment note — both limbs required)?
- If NO consignment note: is this a mere hiring of a goods vehicle, a
  non-GTA transport service, or exempt transport by road other than GTA/courier?
- If GTA: is our entity a "specified recipient" under Notif. 13/2017-CT(R) such
  that RCM at 5% applies?
- Has the vendor opted for forward charge (Annexure V filed by 15 March of the
  preceding FY)? If the invoice carries GST at 18%, what declaration must I hold
  on file to be safe in claiming ITC?
- ITC eligibility on our side and the reasoning.
- The single document I must obtain and retain for this row.

Output: one table, one row per transaction, plus a MISSING DOCUMENTS list.
Flag any row where our treatment differs from what the vendor's invoice implies —
that gap is where the notice comes from.
```

### A3. GTA rate option modelling (5% no-ITC vs 18% with-ITC)

```
We must decide our GTA option for FY <YYYY-YY> before the 15 March deadline.

Inputs I will provide: annual GTA turnover split by customer type (registered
body corporate / factory / partnership / unregistered / composition dealer),
and annual input costs — truck purchases, EMI and interest, tyres, fuel (note:
diesel is outside GST so no ITC), repairs, insurance, GPS/telematics, toll (check
exemption), driver costs, office rent, professional fees.

Build:
1. Scenario A — 5% forward charge, no ITC
2. Scenario B — 18% forward charge, full ITC (Annexure V filed)
3. Scenario C — default RCM at 5%, recipient pays, we claim no ITC

For each: our net GST cost, our effective margin, the customer's landed cost, and
the customer's ITC position. Show a break-even ITC-to-turnover ratio at which
Scenario B beats Scenario A.

Then: which customer segments will resist an 18% invoice and why, whether we can
run a split model across entities or GSTINs, and what the lock-in consequence of
filing Annexure V is (can it be reversed mid-year, and by what form).

Deliver as a one-page decision note addressed to the Board, plus the working table.
```

### A4. Place of supply engine — freight forwarding

```
This is the highest-risk area in our business. For each shipment row I paste,
determine place of supply and tax treatment.

Columns: shipment ref, mode (ocean/air/road/rail), leg (origin-destination),
direction (import / export / cross-trade / domestic), our role (principal vs pure
agent vs agent of carrier), supplier location, recipient location + GSTIN status,
consignee location, whether goods cross a customs frontier, charge heads on the
invoice (ocean freight, THC, BL fee, documentation, CFS, detention, customs duty
disbursement, DO charges).

Determine per row:
1. Nature of supply — inter-state / intra-state / export of service / import of
   service / neither supply (Schedule III) / outside GST.
2. Governing provision — Sec 12(8) where supplier and recipient are both in India,
   Sec 13 where one is outside India. Note that the proviso to 12(8) and Sec 13(9)
   were both omitted with effect from 1 Oct 2023, so transport-of-goods supplies to
   a recipient outside India now fall to the default rule. Apply the post-Oct-2023
   position and say so.
3. Whether export of service qualifies under Sec 2(6) IGST — test all five
   conditions separately, especially the "consideration in convertible foreign
   exchange" and "not merely establishments of a distinct person" limbs.
4. Which charge heads are pure agent recoveries under Rule 33 CGST Rules and which
   are not — apply the three conditions strictly and state the documentation needed.
5. Outbound ocean/air freight: confirm current taxability, noting the export-freight
   exemption entries lapsed on 30 Sep 2022.
6. Import CIF ocean freight RCM: apply Mohit Minerals (SC, 2022) and state the
   consequence.

Output: table + a RISK column rated HIGH/MED/LOW + a separate note on the three
rows most likely to be reopened in a departmental audit.
```

### A5. Warehousing supply characterisation

```
Classify each warehousing contract I describe. The commercial distinction that
matters is bare renting of immovable property versus a composite storage and
warehousing service — they sit in different SACs and behave differently on place
of supply and ITC.

For each contract, determine:
1. Dominant supply and whether it is composite (Sec 2(30)) or mixed (Sec 2(74)).
   Identify the principal supply and justify it from the contract terms, not from
   the invoice label.
2. SAC — distinguish 997212 (renting of immovable property) from the 99672 storage
   and warehousing family (996721 refrigerated, 996722 bulk liquid/gas, 996729
   other storage and warehousing) and from 99671 cargo handling (996711 container
   handling, 996712 CHA, 996713 clearing and forwarding, 996719 other cargo handling).
3. Rate. General storage and warehousing sits at 18% with ITC. Then test whether
   any exemption applies — storage/warehousing of agricultural produce under
   Notif. 12/2017-CT(R) is exempt, but the exemption is confined to produce that
   remains agricultural produce; processed output falls out.
4. Place of supply — if the supply is in relation to immovable property, Sec 12(3)
   drives POS to the property location, which may force a registration in that
   state. If it is a storage service simpliciter, the default rule may apply.
   State which limb you are applying and why.
5. Registration consequence — does this warehouse create a requirement to register
   in that state, and does it constitute a fixed establishment / place of business
   under Sec 2(85)?
6. ITC — on warehouse rent paid, electricity, DG, racking, MHE, forklifts, pallets,
   civil works. Apply Sec 17(5)(c)/(d) to racking and civil works specifically and
   distinguish plant and machinery from works contract / immovable property.
7. Exempt-supply reversal — if any part is exempt (agri produce), compute the
   Rule 42/43 reversal mechanics for common inputs and capital goods.

Output: contract-by-contract table + a REGISTRATION FOOTPRINT summary showing
which states we must be registered in and which are optional.
```

### A6. Cross-charge and distinct-person supplies

```
We operate <n> GSTINs across states, with shared central functions (finance, IT,
HR, insurance, group software licences, senior management).

Build the distinct-person framework:
1. Which shared costs must be cross-charged as supplies between distinct persons
   under Sec 25(4) read with Schedule I para 2, even without consideration.
2. Valuation — Rule 28. When can we invoke the second proviso and treat the
   invoice value as open market value because full ITC is available at the
   recipient end? Test whether every recipient GSTIN is fully ITC-eligible; if any
   has exempt output (agri warehousing), the proviso fails for that leg.
3. Employee cost cross-charge — apply the Northern Operating Systems line of
   reasoning and the subsequent GST circular position on employee secondment and
   HO-BO services. Distinguish salary cost from a service.
4. ISD vs cross-charge — which costs must go through ISD (now mandatory for common
   input services) and which through cross-charge. Give the decision rule.
5. Monthly journal entries and the invoice series to be used.
6. Documentation pack that survives audit.

Output: a policy note plus a monthly cross-charge working template with formulas.
```

### A7. ITC eligibility screen for a logistics fleet

```
Screen our capex and opex lines for ITC under Sec 17(5). Be precise on the
transport carve-outs, because this is where logistics companies over-reverse.

For each line item I paste, output: ELIGIBLE / BLOCKED / PARTIAL / CONDITIONAL,
the exact clause, and the condition to be satisfied.

Cover at minimum:
- Trucks, trailers, tippers, reefer vehicles (goods carriages — note 17(5)(a)
  restricts passenger vehicles of approved seating capacity up to 13, not goods
  carriages)
- Repairs, servicing, insurance and AMC on goods carriages vs on pool cars
- Forklifts, reach trucks, stackers, racking systems, dock levellers
- Warehouse civil construction, flooring, mezzanine, roofing — test 17(5)(c)/(d)
  and the plant-and-machinery exclusion, and note the Safari Retreats position
  and the subsequent legislative response on "plant or machinery"
- Employee transport, canteen, group medical insurance — and where the statutory
  obligation proviso rescues the credit
- Staff welfare, gifts, promotional spend
- Diesel, petrol, CNG (outside/inside GST — state which)
- Toll, parking, FASTag
- Rule 37 — ITC reversal where the vendor is unpaid beyond 180 days: give me the
  ageing query logic to detect these
- Sec 16(4) — the 30 November cut-off for claiming ITC of the prior FY

Output: table + a "top 5 credits we are probably losing unnecessarily" list with
the ₹ estimate method.
```

### A8. GSTR-9 / 9C annual preparation

```
Prepare the annual return working for FY <YYYY-YY>, GSTIN <...>.

Inputs: 12 months of GSTR-1, 12 months of GSTR-3B, 12 GSTR-2B, audited trial
balance, and the books turnover.

Produce:
1. Turnover bridge — audited financials → GSTIN-wise books turnover → GSTR-1 →
   GSTR-3B. Reconcile every step. Common logistics breaks to test for: unbilled
   revenue and job WIP, pure agent recoveries excluded from value, credit notes
   issued after 30 November, exchange-rate differences on export invoices,
   Schedule III items, cross-charges, scrap and tyre sales, and reimbursements
   billed at cost.
2. ITC bridge — books ITC → 3B claimed → 2B available → 9 Table 6 → Table 8.
   Explain Table 8A vs 8B differences by cause, not just by value.
3. Table-wise GSTR-9 fill with the source cell for each figure.
4. GSTR-9C reconciliation with a proposed wording for each unreconciled difference.
5. Additional liability to be paid in DRC-03 with the interest computation.
6. A list of the differences the auditor will ask for evidence on, and what that
   evidence is.

Also flag: is 9C applicable to us at our AATO, and is 9 mandatory or optional.
```

### A9. Notice / ASMT-10 / DRC-01 response drafter

```
Draft a reply to the attached notice.

Step 1 — Parse the notice: issuing authority, section invoked, tax periods, each
para of allegation, quantum per allegation, limitation position under Sec 73 vs 74,
and the reply deadline.

Step 2 — For each allegation, give me:
   (a) what the department is actually saying, in one sentence
   (b) our factual position and the documents that prove it
   (c) the legal position with section/rule/circular, and any favourable ruling —
       mark any case citation UNVERIFIED unless I confirm you have the correct
       citation; do not fabricate one
   (d) merit rating: STRONG / ARGUABLE / WEAK
   (e) if WEAK, the cost of conceding now versus litigating

Step 3 — Draft the reply: formal, para-numbered, mirroring the notice structure,
annexure-referenced, without admissions we do not need to make.

Step 4 — An annexure index listing every document to be attached.

Do not draft the reply until Steps 1 and 2 are complete and I have approved them.
```

### A10. E-way bill & e-invoice control review

```
Audit our e-way bill and e-invoice controls as a logistics operator, where we
generate EWBs both as a supplier and as a transporter on our Transporter ID.

Review and give me a control gap list with severity:
- Part A / Part B completeness and who owns each
- Validity and extension discipline — distance slabs, the over-dimensional cargo
  slab, and the extension window
- Consolidated EWB (GST EWB-02) usage for multi-consignment trips
- Vehicle-number updates on transshipment and breakdown
- Bill-to / ship-to and bill-from / dispatch-from four-party scenarios — noting
  the Ship-To GSTIN field change and the voluntary EWB closure facility introduced
  in 2026, and confirming our system is capturing URP correctly for unregistered
  consignees
- Rejection window and unclosed/expired EWB monitoring
- E-invoice: applicability at our AATO, the IRN reporting time limit for higher-AATO
  taxpayers, IRN-vs-GSTR-1 count reconciliation, QR code on the physical invoice,
  cancellation within the permitted window, and handling of amendments
- EWB-vs-GSTR-1 reconciliation and the discrepancy report the portal shows

Then: a daily/weekly/monthly control calendar with owner, system report name and
tolerance threshold.
```

### A11. Rate & classification change impact

```
A GST rate or classification change has been notified: <paste notification>.

Assess impact on our entity across four axes:
1. Output — which SACs we bill under move, the effective date, and the transition
   rule under Sec 14 (time of supply where rate changes) for shipments in transit
   and for services spanning the change date.
2. Contracts — which customer contracts are GST-inclusive vs exclusive, and where
   a change-in-law clause lets us pass the change through. Draft the customer
   notification email.
3. ITC — whether our input credits are affected, and whether any accumulated
   credit becomes non-utilisable requiring an inverted-duty refund claim under
   Sec 54(3)(ii) with Rule 89(5) formula.
4. Systems — item master, SAC master, e-invoice schema, TMS/WMS rate cards,
   customer rate cards, and the cut-over plan with a dated checklist.

Output: a two-page impact note + a cut-over checklist with owners and dates.
```

### A12. Refund claims

```
Assess and prepare our refund claims for the period <...>.

Evaluate each category we could claim:
- Export of services without payment of tax under LUT — Rule 89(4) formula, the
  adjusted total turnover definition and the export-turnover cap, and the FIRC/BRC
  evidence requirement with the RBI realisation timeline
- Zero-rated supplies to SEZ units/developers — endorsement requirement
- Inverted duty structure — test whether our input/output rate profile qualifies
  and apply Rule 89(5)
- Excess balance in electronic cash ledger
- Tax paid under the wrong head (Sec 77 / Sec 19 IGST)

For the qualifying ones: the computation, the statement to be uploaded, the
document checklist, the two-year limitation start date, and the top three grounds
on which our claim is likely to be deficiency-memo'd — with pre-emptive fixes.
```

---

## SECTION B — MIS & management reporting

### B1. The monthly MIS pack — master build prompt

```
Build the month-end MIS pack for <MM-YYYY>. Audience: MD, CFO, business heads.
Format: one summary page followed by drill-downs. The MD reads only page one.

PAGE 1 — ONE PAGE, NOTHING MORE
- Revenue, gross margin, contribution, EBITDA, PAT: MTD actual / budget / variance
  %, YTD actual / budget / variance %, and same month last year
- Three bullets on what moved, each with the ₹ number attached to the cause
- Cash position, net working capital, DSO, DPO
- Three metrics in the red with named owner and committed recovery date

DRILL-DOWNS
D1. Revenue and margin by business line — road freight / freight forwarding /
    warehousing / VAS. Volume, rate and mix variance decomposed separately.
D2. Top 20 customers — revenue, GM%, DSO, and movement vs prior month. Flag any
    customer whose margin fell more than 300 bps.
D3. Lane / trade-lane profitability — top 15 by volume and the bottom 10 by margin.
D4. Warehouse P&L per facility — revenue per sq ft, cost per sq ft, occupancy %,
    contribution.
D5. Fleet — cost per km, utilisation %, empty-run %, fuel efficiency, maintenance
    cost per vehicle.
D6. Receivables ageing with a provision recommendation under the ECL approach.
D7. Unbilled revenue and accrued vendor cost — the two numbers most likely to be
    wrong; show the movement and the ageing of both.
D8. Compliance dashboard — GST filed/pending, RCM paid, TDS deposited, EWB
    exceptions, ITC at risk.

RULES
- Every variance above ₹<threshold> or <x>% gets a written explanation. No
  unexplained variances.
- Separate volume, rate and mix effects. "Revenue is down because volume is down"
  is not an explanation; quantify each driver.
- Distinguish one-offs from run-rate. Show a normalised EBITDA line.
- No metric without a comparative and a target.
```

### B2. Variance decomposition

```
Decompose the <revenue / gross margin / cost> variance for <period> against
<budget / prior period / prior year>.

Use a stepwise bridge. Sequence and isolate:
  Volume effect → Rate/price effect → Mix effect (customer, lane, mode, service)
  → Cost-input effect (fuel, driver, toll, vendor rate) → FX effect
  → One-off effect → Residual

Rules:
- Residual must be under 2% of the total variance. If it is not, tell me which
  driver you could not isolate and what data you need to isolate it.
- Show the bridge as a waterfall table (step, ₹, cumulative).
- Rank drivers by absolute ₹ impact, not by percentage.
- For each of the top 3 drivers: is it controllable, by whom, and what is the
  specific lever?
- Then, one paragraph in plain language that the MD could read aloud in a
  review meeting.
```

### B3. Flash report — Day 1 close

```
Produce a Day-1 flash estimate for <MM-YYYY> before the books close.

Inputs: operational data only — shipment count and tonnage from TMS, inbound/
outbound lines and storage days from WMS, rate cards, prior three months' actuals,
and known one-offs.

Method:
1. Estimate revenue from operational volumes × contracted rates, adjusted for
   known rate revisions and for the billing lag pattern in the last three months.
2. Estimate direct cost using the rolling cost-per-unit, flexed for fuel price
   movement and any vendor rate change.
3. Estimate fixed cost from the run-rate plus known additions.
4. State the confidence interval on the EBITDA number.
5. List the five assumptions that, if wrong, would move EBITDA by more than
   ₹<x> lakh — these are what we validate first during close.

Then, when actuals are available, I will paste them and you will produce a
flash-vs-actual accuracy report and recommend a correction to the estimation model.
```

### B4. Cash flow forecast — 13-week rolling

```
Build a 13-week direct cash flow forecast.

Inflows: customer collections modelled from the receivables ageing and the actual
realised collection curve by customer bucket (not contractual terms — use observed
behaviour), plus GST refunds expected with realistic portal timelines.

Outflows: vendor payments by category with actual payment behaviour, driver and
staff payroll with statutory dates, fuel (weekly cycle), lease and warehouse rent,
EMI and interest, GST cash payments including RCM which cannot be set off against
credit, TDS and TCS deposits, advance tax instalments, insurance renewals.

Output:
- Week-by-week table with opening balance, receipts by category, payments by
  category, closing balance, and headroom against the sanctioned CC/OD limit
- The weeks where we breach the limit, and by how much
- Three levers ranked by ₹ released per unit of effort and relationship damage
- Sensitivity: what happens if the top customer pays 15 days late
```

### B5. Working capital diagnostic

```
Diagnose our working capital cycle and quantify the cash locked in each stage.

Compute and trend over 12 months: DSO by customer segment and by business line,
unbilled days (job completion to invoice date — this is usually the hidden killer
in freight forwarding), dispute and credit-note ageing, DPO by vendor category,
GST cash blocked (refund receivable + cash ledger balance + ITC not claimable),
security deposits with ports, CFS, airlines and landlords.

For each stage: cash locked in ₹, benchmark for Indian logistics, the gap, and the
specific process change that closes it.

Then rank interventions by ₹ released ÷ weeks to implement, and give me the top
three as a 90-day plan with owners and weekly milestones.
```

### B6. Board / lender pack

```
Draft the quarterly board note for Q<x> FY<YYYY-YY>.

Structure:
1. Executive summary — six bullets, no adjectives
2. Financial performance vs budget with the variance bridge
3. Operating KPIs with trend — volume, yield, utilisation, service levels
4. Customer concentration and contract renewal pipeline with risk flags
5. Working capital and debt covenant compliance — compute each covenant ratio and
   show the headroom
6. Compliance and litigation status — GST, income tax, statutory dues, open notices
   with quantum and provision held
7. Capex status and returns on the last three approved projects
8. Risks, each with owner and mitigation status
9. Decisions sought from the Board, each framed as a specific resolution

Tone: factual, no spin. Where performance is poor, say so in the first line of
that section. A board that discovers a problem from the annexure loses trust in
the pack.
```

### B7. Costing model design

```
Design an activity-based costing model for a company running road freight,
freight forwarding and warehousing.

For each business line, define:
- The cost object (trip / shipment-job / pallet-position-month / outbound line)
- Direct cost pools
- Indirect cost pools and the allocation driver for each, with the reason the
  driver is causally correct rather than convenient
- Contribution definition at each level: Level 1 (revenue − direct third-party
  cost), Level 2 (− attributable fixed), Level 3 (− allocated overhead)
- The reporting hierarchy: shipment → customer → lane → branch → business line

Then: the master data changes required in <ERP> to capture this — new cost centres,
profit centres, job/file numbers, dimension fields — and the data-entry discipline
each one imposes on operations. Be honest about which of these operations will
actually maintain and which will decay within two months.
```

---

## SECTION C — Cargo & freight forwarding job accounting

### C1. Job (file) profitability

```
Compute file-wise profitability for shipment jobs. I will paste the job register.

For each job: job no, customer, mode, trade lane, direction, container type/count
or chargeable weight, revenue heads billed, cost heads accrued, cost heads actual.

Output:
1. Job P&L with gross margin ₹ and %
2. Yield per TEU / per chargeable kg / per trip
3. Cost-accrual accuracy — estimated vs actual by cost head, with the variance %
   and the direction of bias (are we systematically under-accruing?)
4. Jobs open beyond <x> days with no billing — this is unbilled revenue and it is
   where margin quietly dies
5. Jobs with negative margin, each with the probable cause classified as:
   underquoted / cost overrun / uncharged accessorial / detention and demurrage
   not recovered / FX / error
6. Accessorials billed vs incurred — detention, demurrage, CFS, THC, storage,
   amendment fees. Show the recovery rate per head.

Close with: the three recurring leakage patterns and the control that stops each.
```

### C2. Freight forwarding revenue recognition

```
Set our revenue recognition policy for freight forwarding under <Ind AS 115 / AS 9>.

Address:
1. Principal vs agent — apply the control indicators to each service. Where we are
   agent, revenue is the net commission; where principal, gross. Go head by head:
   ocean freight, air freight, customs clearance, transport leg, documentation,
   insurance arranged for the customer.
2. Performance obligation and timing — is freight revenue earned at a point in time
   (departure) or over time (transit)? Justify by reference to the transfer-of-
   control criteria, and state the practical policy (many Indian forwarders use
   departure or completion; pick one and defend it).
3. Month-end cut-off — the specific rule for shipments in transit at month end, and
   the matching accrual for the corresponding vendor cost.
4. Disbursements and pure-agent recoveries — accounting treatment vs GST treatment.
   These diverge and the divergence must be documented, because it is exactly the
   gap a GST turnover reconciliation surfaces.
5. Journal entries for a complete job lifecycle: quote → booking → cost accrual →
   customer invoice → vendor invoice → accrual reversal → settlement, including the
   FX revaluation entries.

Output: a policy document plus a worked example with full entries.
```

### C3. FX on international freight

```
Build the FX control framework for our forwarding business.

Cover:
1. Rate to be used for recording a foreign-currency invoice — the GST rule (rate
   notified under Sec 14 of the Customs Act for supply of goods; the generally
   accepted accounting principle rate for services) versus the accounting standard
   requirement. These are not the same rate and the difference must be booked
   consistently.
2. Monthly revaluation of foreign-currency receivables and payables, and where the
   gain/loss goes.
3. Whether exchange differences form part of GST turnover, and the impact on the
   GSTR-9 turnover bridge.
4. Realised vs unrealised segregation for tax purposes and the Sec 43AA / ICDS VI
   position.
5. Overseas agent balances — netting agreements, the FEMA position on netting, and
   the documentation required.
6. A monthly FX exposure report format showing net open position by currency.

Output: policy + monthly working template + the three entries most commonly booked
wrong at Indian forwarders.
```

### C4. Detention, demurrage & accessorial recovery

```
Analyse our detention and demurrage recovery for <period>.

Inputs: charges levied by shipping lines/CFS/airlines, charges billed onward to
customers, and free-time terms by customer contract.

Produce:
1. Recovery rate by customer, by lane, by charge head
2. ₹ absorbed by us with the root cause classified as: customer delay / our
   operational delay / documentation error / customs hold / contractual free-time
   mismatch / not billed at all
3. The customers where our contracted free time is shorter than what we pass on —
   a structural loss that repeats every shipment
4. Recovery ageing and the point at which recovery probability collapses
5. A one-page recovery playbook: who bills, within how many days, with what
   evidence pack, and the escalation ladder
6. Contract clause language to fix the structural gaps
```

---

## SECTION D — Warehouse & 3PL

### D1. Warehouse P&L and unit economics

```
Build the per-facility warehouse P&L and unit economics for <facility>.

Revenue: storage (per pallet-position-month or per sq ft), inbound handling,
outbound handling, VAS, minimum guarantee billing, SLA penalties (negative).

Cost: rent, CAM, electricity, DG, manpower (own vs contracted), MHE lease and
maintenance, packaging consumables, security, housekeeping, insurance, technology,
allocated overhead.

Compute:
- Revenue per sq ft and per pallet position
- Cost per sq ft, per pallet position, per inbound line, per outbound line
- Occupancy % (positions occupied / positions available) and throughput per sq ft
- Break-even occupancy % — the number the business head should have tattooed
- Contribution margin and the ₹ impact of each 5-point occupancy change
- Minimum guarantee utilisation — are we billing the MG floor where actual volume
  falls short, and is the clause enforceable as drafted?

Then: a ranked list of the five levers with the largest ₹ impact, separated into
pricing levers and cost levers.
```

### D2. Inventory accountability — custodial stock

```
We hold customer-owned inventory as a bailee, not as owner. Set the accounting and
control framework.

Cover:
1. Accounting treatment — customer stock stays off our balance sheet; the
   disclosure and the memorandum records required.
2. GST — movement of goods to and from our warehouse on delivery challan under
   Rule 55 where there is no supply; when a delivery challan is sufficient and when
   a tax invoice is required; e-way bill for non-supply movements; whether our
   warehouse address must be added as an additional place of business on the
   customer's GSTIN and the consequence if it is not.
3. Shrinkage and damage — the liability recognition trigger, the measurement basis
   (customer cost vs selling price vs contract cap), the insurance claim interplay,
   and the GST treatment of a compensation payment (is it consideration for
   tolerating an act — apply the current circular position on liquidated damages).
4. Physical verification protocol — cycle count frequency by ABC class, full count
   cadence, tolerance thresholds, the sign-off matrix, and what constitutes an
   exception that must escalate to the CFO.
5. Reconciliation — WMS stock vs customer ERP stock vs physical count, three-way,
   with a defined break-resolution SLA.
6. The month-end certificate the customer will ask for, and what we should and
   should not certify.
```

### D3. 3PL contract commercial review

```
Review this 3PL warehousing contract from a finance and tax standpoint.

Assess and flag:
1. Rate card completeness — is every activity we perform priced? List the
   unpriced activities, because those are pure cost.
2. Minimum guarantee — floor level, measurement period, carry-forward, and whether
   it is enforceable as drafted
3. Escalation clause — index, frequency, cap, and whether it actually covers our
   real cost inflation (rent, minimum wages, electricity, fuel)
4. SLA penalties — cap, measurement method, dispute mechanism, and the worst-case
   annual exposure computed
5. Payment terms, credit period, interest on delay, and whether the interest clause
   is one-way
6. GST — who is the recipient, the place of supply consequence for our registration
   footprint, whether prices are inclusive or exclusive, and the change-in-law
   pass-through
7. Exit — notice period, transition obligations, asset treatment on exit,
   stranded-cost recovery. Exit clauses are where 3PL contracts destroy value.
8. Liability cap vs our insurance cover — quantify the uninsured gap in ₹

Output: a clause-by-clause table (clause / issue / risk rating / suggested redraft),
plus the three clauses I should refuse to sign without change.
```

### D4. MHE and racking capex appraisal

```
Appraise this warehouse capex proposal: <details>.

Build:
1. Cash flow model over the asset life with the correct tax shield — depreciation
   under the Income-tax Act (WDV block rates) separate from book depreciation under
   the Companies Act Schedule II useful lives
2. GST ITC treatment of the capex and the timing of the credit; test whether
   racking and civil works fall foul of Sec 17(5)(c)/(d), and apply the current
   "plant or machinery" position
3. Lease vs buy comparison, with the Ind AS 116 balance-sheet consequence of the
   lease route (ROU asset, lease liability, and the effect on our debt covenants —
   check whether the covenant definition includes lease liabilities)
4. NPV, IRR, payback and discounted payback at our WACC of <x>%
5. Sensitivity on occupancy, contract tenure vs asset life mismatch (the key risk —
   a 7-year asset on a 3-year contract), and residual value
6. The break-even contract tenure

Output: decision memo with a clear recommendation and the single assumption the
decision is most sensitive to.
```

---

## SECTION E — Direct tax, payroll & statutory

### E1. TDS/TCS engine for logistics

```
Build our TDS/TCS decision matrix for the payment types a logistics company makes.

For each: section, rate, threshold (single + aggregate), PAN-missing rate,
deposit due date, return quarter, and the specific trap.

Cover at least:
- Transport contractors — Sec 194C, and the nil-deduction route under 194C(6) for
  a transporter owning not more than ten goods carriages: the declaration format,
  the PAN requirement, and the mandatory reporting of these payments in the TDS
  return even though no tax is deducted. This is the single most commonly botched
  item in Indian transport accounting.
- Warehouse rent — 194I, and the distinction between rent for premises and a
  composite storage service taxable under 194C or 194J
- Professional and technical services — 194J, including the lower rate limb
- Commission and brokerage — 194H, and overseas agent commission
- Purchase of goods — 194Q, and its interaction with TCS under 206C(1H); state the
  priority rule
- Payments to non-residents — 195, with the Sec 9 source rule, treaty relief,
  Form 15CA/15CB requirement, TRC and Form 10F, and the equalisation levy /
  significant economic presence position on digital payments
- Ocean freight payments to foreign shipping lines — Sec 172 vs 194C vs 195, and
  the certificate mechanism

Output: a matrix table + a monthly TDS close checklist + the top five items that
show up as short-deduction defaults in the TRACES justification report.
```

### E2. Tax audit & return readiness

```
Prepare the tax audit readiness file for FY <YYYY-YY>.

1. Applicability — turnover-based threshold and the effect of the digital-receipts
   proviso; confirm whether 44AB applies and under which clause.
2. Form 3CD clause-wise working paper index, with the source report for each clause.
   Focus on the ones that bite in logistics: 21 (inadmissible expenditure), 26
   (Sec 43B including the MSME payment disallowance under 43B(h)), 27 (CENVAT/ITC),
   31 (loans and deposits), 34 (TDS compliance), 44 (expenditure break-up by GST
   registration status — this must tie to the GST returns).
3. Sec 43B(h) MSME exposure — the vendor-classification process, the 15/45-day
   test, the year-end ageing extract logic, and the disallowance computation.
4. Book-to-tax reconciliation with each adjustment mapped to a section.
5. Depreciation: Companies Act vs Income-tax Act, block-wise, with the
   additional-depreciation and put-to-use tests for vehicles purchased late in the
   year.
6. Advance tax and interest under 234B/234C computation with the instalment-wise
   shortfall.
7. The ten questions the tax auditor will ask, and the file reference that answers
   each.
```

### E3. Payroll & driver compliance

```
Review our payroll and contract-labour compliance for a transport and warehousing
workforce.

Cover: PF (including international worker rules if any), ESIC, professional tax
state-wise, minimum wages by state and by skill category, bonus, gratuity
provisioning (actuarial vs simple), leave encashment, and the Motor Transport
Workers Act obligations for drivers.

For contract labour at warehouses: principal-employer liability under CLRA, the
PF/ESIC compliance evidence we must collect from the contractor each month before
releasing payment, the specific documents, and the exposure if the contractor
defaults.

Driver-specific: overtime and duty-hour records, trip allowance versus salary and
its taxability, night allowance, and whether our trip-allowance structure survives
scrutiny as reimbursement rather than salary.

Output: a compliance calendar by state, a monthly evidence checklist, and a
quantified exposure estimate for the top three gaps.
```

---

## SECTION F — Automation & tooling prompts

### F1. Reconciliation script

```
Write a Python script (pandas) that reconciles GSTR-2B against our purchase register.

Inputs: two Excel files with columns <paste actual column names from both files>.

Requirements:
- Normalise GSTIN (strip, upper), invoice number (remove leading zeros, slashes,
  spaces, case), and date (handle DD-MM-YYYY, DD/MM/YY, and Excel serials)
- Match in cascading passes: (1) exact GSTIN+invoice+date+value,
  (2) GSTIN+invoice+value, (3) GSTIN+invoice with value tolerance ±₹2 (rounding),
  (4) fuzzy invoice number within same GSTIN and same month, score >= 85
- Output one Excel workbook with sheets: MATCHED, IN_2B_NOT_IN_BOOKS,
  IN_BOOKS_NOT_IN_2B, VALUE_MISMATCH, PROBABLE_MATCH_REVIEW, SUMMARY
- SUMMARY sheet: counts and ₹ by bucket, ITC at risk, and supplier-wise rollup of
  IN_BOOKS_NOT_IN_2B sorted by value descending
- Conditional formatting on the mismatch sheets
- Log every normalisation applied so a reviewer can audit the matching

Add docstrings and a --dry-run flag. No external API calls.
```

### F2. Power Query / Excel model

```
Give me the Power Query M code and the Excel model structure for a self-refreshing
warehouse MIS.

Sources: WMS export (inbound/outbound/storage transactions), Tally daybook export,
rate card table, facility master.

Deliver:
1. M code for each query with the transformation steps commented
2. The star schema: fact tables (transactions, billing) + dimensions (facility,
   customer, date, activity)
3. DAX measures for: revenue per sq ft, occupancy %, cost per outbound line,
   contribution %, MTD/YTD/PY comparatives with correct time intelligence
4. The pivot layout for the MIS page
5. A refresh runbook a junior accountant can follow without help

Keep the model under <x> MB and explain any step that will break if the source
column order changes.
```

### F3. SQL for the operational data layer

```
Write SQL (<dialect>) against our TMS/WMS schema <paste DDL> to produce:

1. Lane-wise margin — origin, destination, mode, trips, tonnage, revenue, direct
   cost, contribution, contribution %, ranked
2. Unbilled jobs — jobs delivered more than <x> days ago with no customer invoice
3. Vendor accrual accuracy — accrued vs actual by cost head and by vendor
4. EWB exceptions — shipments with no EWB, expired EWB, or Part B not updated
5. Detention/demurrage — incurred vs recovered by customer

Requirements: CTEs not nested subqueries, explicit joins, no SELECT *, parameterised
date range, and a comment above each CTE saying what it isolates. Add the indexes
you would create and why.
```

### F4. Excel formula and template builder

```
I need <describe the working paper>.

Give me:
1. The sheet structure — Input / Working / Output, with the rule that no cell in
   Output contains a hardcoded number
2. Exact formulas, using structured references or named ranges, not A1 chains
3. Input validation and error trapping on every input cell
4. A control-total row that ties to the source, with a visible TIE/BREAK flag
5. A reviewer's tick-mark column and a change log sheet
6. Protection scheme — which cells locked, which open

Rule: any formula longer than one line must be broken into helper columns. A
working paper nobody can review is a working paper that hides errors.
```

---

## SECTION G — Verification & challenge prompts

Run at least one of these after any substantive tax or numeric output. This is the layer most people skip and it is the one that catches the expensive errors.

### G1. Adversarial review

```
Re-read your previous answer as a GST officer conducting an audit of our entity,
whose incentive is to raise a demand.

For each position taken:
- The strongest argument against it
- The evidence the officer would ask for, and whether we would have it
- The quantum of demand plus interest under Sec 50 and penalty under Sec 122/73/74
- Whether the extended period under Sec 74 could be invoked, and on what allegation
- A revised risk rating: HIGH / MEDIUM / LOW

Then tell me which positions I should change now, and which I should keep but
document more heavily.
```

### G2. Numbers integrity check

```
Audit your own computation.

1. Recompute every figure independently and state whether it agrees
2. Check every cross-total, subtotal and percentage
3. List every assumption you made that I did not give you, and flag any that
   materially drive the result
4. State the three figures with the widest uncertainty band and why
5. State what you could NOT verify from the data provided

If anything fails, say FAIL and show the corrected figure. Do not restate the
answer as if it were right.
```

### G3. Citation verification

```
List every statutory reference, notification, circular and case citation in your
previous answer.

For each, mark:
- VERIFIED — you are confident of the exact number, date and that it remains in force
- UNCERTAIN — the proposition is right but you are not sure of the exact reference
- UNVERIFIED — do not rely on this until I check the source

Then give me the precise search string to verify each UNCERTAIN and UNVERIFIED item
on cbic-gst.gov.in or the GST portal. An invented citation in a reply to a notice
is worse than no citation at all — be conservative.
```

---

## SECTION H — Claude Skills to build

These are `SKILL.md` capability packs. Each lives in its own folder with supporting scripts, templates and reference files, and triggers automatically when the work matches. Build them in the order listed — the first three carry most of the value.

### H1. `gst-logistics`

```yaml
---
name: gst-logistics
description: >
  Use when working on GST for a transport, freight forwarding, courier, CHA or
  warehousing business. Covers GTA classification and the 5%/18%/RCM option,
  place of supply for freight under Sec 12(8) and Sec 13, pure agent recoveries
  under Rule 33, warehousing SAC classification, ITC screening under Sec 17(5)
  for fleets and warehouses, e-way bill and e-invoice controls, and GSTR-1/3B/2B/
  IMS/9/9C preparation. Trigger on: GTA, consignment note, LR, bilty, freight
  forwarding, ocean freight, THC, detention, demurrage, CFS, warehousing, 3PL,
  e-way bill, RCM, GSTR, IMS, place of supply.
---
```

Bundle inside the folder:
- `reference/gta-decision-tree.md` — the full classification flow with the Annexure V mechanics
- `reference/place-of-supply-matrix.md` — every freight scenario, post-Oct-2023 position
- `reference/sac-master-logistics.csv` — 9965 / 9966 / 9967 families with rates and notes
- `reference/blocked-credit-17-5.md` — annotated for transport and warehousing
- `reference/compliance-calendar.md` — dated, by return type and turnover slab
- `scripts/recon_2b_vs_pr.py` — the reconciliation from F1
- `scripts/rcm_register.py` — RCM computation and self-invoice generator
- `templates/annexure-v.docx`, `templates/self-invoice.docx`, `templates/pure-agent-declaration.docx`
- `templates/194c6-transporter-declaration.docx`

### H2. `mis-close`

```yaml
---
name: mis-close
description: >
  Use for month-end close, management reporting and variance analysis in a
  logistics business. Produces the MIS pack, decomposes variance into volume,
  rate, mix and cost-input effects, builds flash estimates, and runs the close
  checklist. Trigger on: month-end, MIS, close, variance, flash, management
  reporting, board pack, EBITDA bridge, budget vs actual.
---
```

Bundle:
- `reference/close-checklist.md` — Day 0 to Day 5, task, owner, dependency
- `reference/kpi-definitions.md` — every metric defined once, with the formula and the data source, so nobody redefines DSO mid-year
- `reference/variance-method.md` — the decomposition sequence and the residual rule
- `scripts/build_mis.py` — assembles the pack from the standard extracts
- `templates/mis-pack.xlsx`, `templates/board-note.docx`

### H3. `job-costing-freight`

```yaml
---
name: job-costing-freight
description: >
  Use for shipment-level and file-level profitability in freight forwarding and
  road transport — job P&L, yield per TEU or per chargeable kg, accrual accuracy,
  unbilled job detection, accessorial recovery, and FX on international freight.
  Trigger on: job costing, file profitability, shipment margin, unbilled, accrual,
  detention recovery, yield per TEU, lane margin.
---
```

Bundle:
- `reference/charge-head-master.md` — every revenue and cost head with the accounting and GST treatment side by side
- `reference/principal-vs-agent.md` — the control test applied head by head
- `scripts/job_pnl.py`, `scripts/unbilled_detector.py`
- `templates/job-pnl.xlsx`

### H4. `warehouse-economics`

```yaml
---
name: warehouse-economics
description: >
  Use for warehouse and 3PL financial work — per-facility P&L, cost per pallet
  position and per line, occupancy and break-even analysis, minimum guarantee
  billing, SLA penalty exposure, custodial stock accounting and shrinkage, MHE
  capex appraisal, and 3PL contract commercial review. Trigger on: warehouse P&L,
  cost per sq ft, pallet position, occupancy, 3PL contract, minimum guarantee,
  shrinkage, MHE, racking.
---
```

Bundle:
- `reference/unit-economics.md` — every warehouse metric with the formula
- `reference/contract-review-checklist.md` — the clause list from D3
- `scripts/warehouse_pnl.py`, `scripts/breakeven_occupancy.py`
- `templates/facility-pnl.xlsx`, `templates/capex-appraisal.xlsx`

### H5. `tax-notice-response`

```yaml
---
name: tax-notice-response
description: >
  Use when responding to a GST or income-tax notice, ASMT-10, DRC-01, DRC-01B,
  scrutiny query or audit observation. Parses the notice, builds an allegation-wise
  merit and evidence analysis, and drafts the reply with an annexure index.
  Trigger on: notice, ASMT-10, DRC-01, DRC-03, scrutiny, show cause, SCN,
  departmental audit, adjudication.
---
```

Bundle:
- `reference/reply-structure.md` — the para-numbered format
- `reference/limitation-map.md` — Sec 73 vs 74 timelines and the consequences
- `reference/citation-discipline.md` — the rule that no citation goes in unverified
- `templates/reply-to-scn.docx`, `templates/annexure-index.xlsx`

### H6. `working-paper`

```yaml
---
name: working-paper
description: >
  Use when building any Excel working paper, schedule or reconciliation that a
  reviewer or auditor will sign off. Enforces Input/Working/Output separation,
  no hardcoded numbers in output, control totals, tick marks, and a change log.
  Trigger on: working paper, schedule, reconciliation template, audit schedule,
  tie-out.
---
```

Bundle:
- `reference/wp-standards.md` — the house rules
- `scripts/wp_scaffold.py` — generates a compliant workbook shell
- `templates/wp-shell.xlsx`

---

## SECTION I — Domain cheat-sheet

Current position as at September 2026. **Verify before relying on any line.**

### GST — transport & logistics

| Item | Position |
|---|---|
| GTA definition | Transport of goods by road **and** issue of a consignment note. Both limbs. No consignment note → not a GTA. |
| GTA rate options | 5% without ITC, or 18% with full ITC under forward charge. The earlier 12% forward-charge slab was rationalised to 18% with effect from 22 Sep 2025. |
| GTA RCM | Where the GTA has not opted for forward charge, a specified recipient under Notif. 13/2017-CT(R) pays 5% under RCM. Specified recipients include a registered person, factory, society, co-operative society, body corporate, partnership firm/AOP and casual taxable person. |
| Forward-charge opt-in | Annexure V, filed on the portal by 15 March of the preceding FY. Annexure VI to revert. Default position absent any filing is RCM at 5%. |
| RCM payment | Must be discharged in cash. Cannot be set off against ITC. |
| Freight forwarding / courier | 18%. |
| Warehousing & storage (general) | 18% with ITC — SAC family 99672. |
| Agricultural produce storage | Exempt under Notif. 12/2017-CT(R). Confined to produce that remains agricultural produce; processed output falls out. |
| Export freight (outbound ocean/air) | The exemption entries lapsed on 30 Sep 2022. Taxable. |
| Import CIF ocean freight RCM | Struck down — *Mohit Minerals*, Supreme Court, May 2022. |
| Place of supply — freight | Sec 12(8) where both parties are in India; Sec 13 where one is outside India. The proviso to 12(8) and Sec 13(9) were both omitted with effect from 1 Oct 2023. |
| Goods-vehicle ITC | Sec 17(5)(a) restricts passenger vehicles of approved seating capacity up to 13. Goods carriages are not blocked; nor are their repairs and insurance. |
| ITC time limit | Sec 16(4) — 30 November of the following FY, or filing of the annual return, whichever is earlier. |
| Rule 37 | Reverse ITC where the vendor is unpaid beyond 180 days from the invoice date. Reclaim on payment. |
| GSTR-3B liability | Auto-populated from GSTR-1 / GSTR-1A / IFF and non-editable since the July 2025 period. Corrections route through GSTR-1A, which can be filed only once per period, before 3B. |
| GSTR-3B ITC (Table 4) | Locking of auto-populated ITC has been signalled for around the July 2026 period, with ITC tied to GSTR-2B and IMS actions. Confirm the go-live against the current GSTN advisory before changing filing practice. |
| IMS | Accept / Reject / Pending on each inward record. **No action = deemed acceptance.** Review before GSTR-2B generation on the 14th. |
| E-invoicing | Mandatory above ₹5 crore AATO. Higher-AATO taxpayers face a 30-day IRN reporting limit from the invoice date. |
| E-way bill | Required above ₹50,000 consignment value. Ship-To GSTIN is now a mandatory field for Bill-To/Ship-To (enter URP for an unregistered consignee), alongside a voluntary EWB closure facility — both effective 1 Aug 2026 after deferral from 15 June. |

### Metrics that matter

**Freight:** revenue per trip · cost per km · empty-run % · vehicle utilisation % · yield per TEU · yield per chargeable kg · lane contribution % · accrual accuracy % · unbilled days

**Warehouse:** revenue per sq ft · cost per pallet position · occupancy % · break-even occupancy % · throughput per sq ft · dwell time · inventory accuracy % · shrinkage as % of throughput value · cost per outbound line · lines per man-hour

**Finance:** DSO · unbilled days · DPO · cash conversion cycle · GST cash blocked · ITC at risk · detention recovery % · gross margin by business line · normalised EBITDA

---

## SECTION J — Guardrails

1. **The model drafts; the CA signs.** Nothing here substitutes for professional judgement or for a signed opinion. Every tax position goes to a qualified reviewer before it leaves the building.
2. **Never let a citation through unverified.** An invented notification number in a reply to a show-cause notice damages credibility permanently. Use G3 every time.
3. **Never paste client PII, GSTIN-linked transaction dumps or customer contracts into a general model** unless your organisation's data policy permits it. Anonymise: replace GSTINs with tokens, customer names with codes, and keep the mapping offline.
4. **Rates and thresholds drift.** Anchor to a notification, not to a memory. Re-verify at the start of every FY and after every Council meeting.
5. **Recompute anything that goes into a return.** Model arithmetic on long tables is not reliable enough to file on. Recompute in Excel or in a script and tie the totals.
6. **Keep the audit trail.** The prompt and its output are a working paper. Save both with the date, the source data reference and the reviewer's name.
