# 🌱 AIA Modelling Best Practices

These best practices were distilled from a complete AIA modelling exercise of a grid-connected renewable electricity project under ACM0002 (see 2-ecoregistry125/).

---

## 1 Model Impacts as State Differences, Not Just Numbers

**Principle:**

An impact is a change between two states, not a standalone numeric value.

**Best practice:**

- Always create:
  - Two `impactont:State` nodes
  - One `impactont:Impact` node
    linking them via:
  - `impactont:hasStateA`
  - `impactont:hasStateB`
- Never model emission reductions as a single literal.

---

## 2 Distinguish Causal Provenance from Quantification Source

What caused the state? `impactont:hasProvenance` (causal provenance)
Where did the numeric value come from? `dcterms:source` (epistemic/measurement provenance)

**Best practice:**

- Attach `impactont:hasProvenance` to a state via a `StateProvenanceClaim`
- Attach `dcterms:source` to the quantified value of the state, as an `impactont:IndicatorValue` node (via the `impactont:hasIndicatorValue` property of an `impactont:State` node)

Never mix these two.

---

## 3 Always Reify Indicator Values (If They Have Metadata)

If a value has:

- A source
- A methodology
- Uncertainty
- A calculation activity
- Or verification linkage

→ It must be an `impactont:IndicatorValue` node, not a bare literal.

Pattern:

State\
└─ impactont:hasIndicatorValue → impactont:IndicatorValue\
  ├─ rdf:value\
  └─ dcterms:source

---

## 4 Use `StateProvenanceClaim` and `ImpactClaim` for Causality

Do not attach causal properties directly to states if you want claims to be accountable.

StateProvenanceClaim\

- subject = State\
- predicate = impactont:hasProvenance\
- object = Event

ImpactClaim\

- subject = Impact (state difference)\
- predicate = impactont:hasProvenance\
- object = Event

---

## 5 Use One Project Node as the Container

Everything real that occurred within the project should be connected via:

aiao:Project\
└─ aiao:comprisesActivity → Activity

Do not include counterfactual baseline activities in the project container.

---

## 6 Use Agent-Role-Activity Relations for Accountability

Never attach roles directly to agents.

Always use:

AgentActivityRelation\
├─ aiao:hasAgent\
├─ aiao:hasActivity\
└─ aiao:isGovernedBy → Role

---

## 7 Always Add Claimants

Every claim should have:

`claimont:hasClaimant` → Agent

Impact accounting without a claimant is incomplete.

---

## 8 Temporal Hygiene

- Use `aiao:hasTemporalLocation` consistently.
- Type all instants and intervals also as `impactont:TemporalLocation`.
- Avoid using `dcterms:temporal` for activity timing in AIA graphs.

---

## 9 Use Schema.org Pragmatically (But Carefully)

Use `schema:` only when:

- ImpactOnt does not define the concept (e.g., location, address, identifiers)
- It adds interoperability value

Do not replace AIA semantics with schema semantics.

---

# 🔎 Structural Integrity Checklist

Before finalizing a project graph, verify:

- [ ] Every impact has two states
- [ ] Every state has an indicator
- [ ] Every indicator value is sourced
- [ ] Every claim has a claimant
- [ ] Every causal statement is modelled as a claim
- [ ] Every real activity belongs to a project
- [ ] Counterfactual activity does NOT belong to the project
- [ ] Accountability is expressed via AgentActivityRelation
- [ ] Documents are linked to claims
- [ ] Temporal modelling uses AIA properties

---

# 🌍 Modelling Layers

AIA modelling distinguishes three layers:

1.  **World layer** --- Things, States, Events\
2.  **Epistemic layer** --- Claims, Values, Sources\
3.  **Accountability layer** --- Agents, Roles, Relations

Keeping these layers separate is the key to clean impact accounting.
