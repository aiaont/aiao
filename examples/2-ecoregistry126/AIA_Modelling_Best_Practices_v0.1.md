# 🌱 AIA Modelling Best Practices (Draft v0.1)

These best practices are distilled from a complete AIA modelling
exercise of a grid-connected renewable electricity project under
ACM0002.

------------------------------------------------------------------------

## 1️⃣ Model Impacts as State Differences --- Not Just Numbers

**Principle:**\
An impact is a change between two states, not a standalone numeric
value.

**Best practice:** - Always create: - Two `impactont:State` nodes - One
`impactont:Impact` node linking them via: - `impactont:hasStateA` -
`impactont:hasStateB` - Never model emission reductions as a single
literal.

------------------------------------------------------------------------

## 2️⃣ Distinguish Causal Provenance from Quantification Source

  ---------------------------------------------------------------------------
  Concept                 Property                    Meaning
  ----------------------- --------------------------- -----------------------
  What caused the state?  `impactont:hasProvenance`   Causal provenance

  Where did the numeric   `dcterms:source`            Epistemic/measurement
  value come from?                                    provenance
  ---------------------------------------------------------------------------

**Best practice:** - Attach `impactont:hasProvenance` via a
`StateProvenanceClaim` - Attach `dcterms:source` to the reified value
node

Never mix these two.

------------------------------------------------------------------------

## 3️⃣ Always Reify Indicator Values (If They Have Metadata)

If a value has: - A source - A methodology - Uncertainty - A calculation
activity - Or verification linkage

→ It must be a node, not a bare literal.

Pattern:

State\
└─ impactont:hasIndicatorValue → ValueNode\
  ├─ rdf:value\
  └─ dcterms:source

------------------------------------------------------------------------

## 4️⃣ Explicitly Model Counterfactual States

Baseline ≠ Real.

**Best practice:** - Use `impactont:hasModality` - `"real"` -
`"counterfactual"` - Reflect this in node names.

------------------------------------------------------------------------

## 5️⃣ Use `StateProvenanceClaim` and `ImpactClaim` for Causality

Do not attach causal properties directly to states if you want claims to
be accountable.

StateProvenanceClaim\
- subject = State\
- predicate = impactont:hasProvenance\
- object = Event

ImpactClaim\
- subject = Impact (state difference)\
- predicate = impactont:hasProvenance\
- object = Event

------------------------------------------------------------------------

## 6️⃣ Use One Project Node as the Container

Everything real that occurred within the project should be connected
via:

aiao:Project\
└─ aiao:comprisesActivity → Activity

Do not include counterfactual baseline activities in the project
container.

------------------------------------------------------------------------

## 7️⃣ Use Agent--Role--Activity Relations for Accountability

Never attach roles directly to agents.

Always use:

AgentActivityRelation\
├─ aiao:hasAgent\
├─ aiao:hasActivity\
└─ aiao:isGovernedBy → Role

------------------------------------------------------------------------

## 8️⃣ Always Add Claimants

Every claim should have:

claimont:hasClaimant → Agent

Impact accounting without a claimant is incomplete.

------------------------------------------------------------------------

## 9️⃣ Treat Documents as Evidence, Not Claims

Documents should be typed as:

`dcterms:Resource`

Claims should reference documents via:

`claimont:isSupportedBy → Document`

Do not collapse document, claim, and activity layers.

------------------------------------------------------------------------

## 🔟 Temporal Hygiene

-   Use `aiao:hasTemporalLocation` consistently.
-   Type all instants and intervals also as
    `impactont:TemporalLocation`.
-   Avoid using `dcterms:temporal` for activity timing in AIA graphs.

------------------------------------------------------------------------

## 11️⃣ Avoid Redundant Typing

If:

`aiao:Activity rdfs:subClassOf impactont:Event`

Do not assert both types on instances.

------------------------------------------------------------------------

## 12️⃣ Use Schema.org Pragmatically (But Carefully)

Use `schema:` only when: - ImpactOnt does not define the concept (e.g.,
location, address, identifiers) - It adds interoperability value

Do not replace AIA semantics with schema semantics.

------------------------------------------------------------------------

# 🔎 Structural Integrity Checklist

Before finalizing a project graph, verify:

-   [ ] Every impact has two states
-   [ ] Every state has an indicator
-   [ ] Every indicator value is sourced
-   [ ] Every claim has a claimant
-   [ ] Every causal statement is modelled as a claim
-   [ ] Every real activity belongs to a project
-   [ ] Counterfactual activity does NOT belong to the project
-   [ ] Accountability is expressed via AgentActivityRelation
-   [ ] Documents are linked to claims
-   [ ] Temporal modelling uses AIA properties

------------------------------------------------------------------------

# 🌍 Philosophical Principle Behind AIA

AIA modelling distinguishes three layers:

1.  **World layer** --- Things, States, Events\
2.  **Epistemic layer** --- Claims, Values, Sources\
3.  **Accountability layer** --- Agents, Roles, Relations

Keeping these layers separate is the key to clean impact accounting.
