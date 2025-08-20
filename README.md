# AIAO: Anthropogenic Impact Accounting Ontology

## Overview

The Anthropogenic Impact Accounting Ontology (AIAO) provides a semantic framework for accounting for human impact on environments. This ontology is the product of collaboration by members of the Standards Working Group of the LFDT CA2-SIG. It is one of four ontologies.

## Linked Ontologies

AIAO is closely integrated with three complementary ontologies:

- **[Impact Ontology](http://w3id.org/impactont)**: Provides a foundational semantic framework for impact modeling, such as the impact of some human activity on its environment.

- **[Claim Ontology](http://w3id.org/claimont)**: Provides a foundational semantic framework for representing claims and assertions, such as those made about an agent's activities.
  
- **[Information Communication Ontology](http://w3id.org/infocomm)**: Provides a foundational semantic framework for representing information communication aspects, e.g., those related to impact reporting.

Together, these four ontologies comprise a suite of ontologies for environmental and social impact accounting.

## Core Classes

### Agent (`aiao:Agent`)

Represents any entity that can bear accountability for the occurrence of an activity. This includes:

- Natural persons
- Legal persons (companies, organizations)
- Cyber personas (digital identities)

Agents may comprise other agents, allowing for complex organisational structures.

### Activity (`aiao:Activity`)

An event orchestrated by an agent that impacts an environment. Activities are the primary units of impact generation.

### Environment (`aiao:Environment`)

The surroundings and conditions in which a thing occurs. This encompasses, inter alia:

- Physical environments
- Natural environments
- Social environments
- Political environments
- Economic environments
- Legal environments

### Control (`aiao:Control`)

Rules, plans, or constraints that limit or direct activities or things. These may include:

- Implementation plans
- Monitoring plans
- Methodologies
- Objectives and goals

### Claims

Two types of claims capture different aspects of impact:

- **ImpactClaim (`aiao:ImpactClaim`)**: A claim about the causal relation between an event and a state difference

- **StateClaim (`aiao:StateClaim`)**: A claim about the state of some thing

## Key Relationships

### Agent-Activity Relations

The ontology models complex relationships between agents and activities through:

- `hasAgentActivityRelation`: Links activities to their agents
- `hasAgent`: Identifies the agent in an agent-activity relation
- `hasActivity`: Identifies the activity in an agent-activity relation
- `governs`: Describes how roles control agent-activity relation

### Activity Controls

Activities are governed by various controls:

- `hasObjective`: Links activities and projects to their goals
- `isPerformedWith`: Connects activities to their instruments
- `comprisesActivity`: Allows projects to encompass multiple activities

## Usage

AIAO is particularly suited for:

- Environmental impact assessment
- Corporate sustainability reporting
- Carbon accounting and offset projects
- Social impact measurement
- Regulatory compliance tracking

## Namespace

The ontology uses the namespace `http://w3id.org/aiao#` and is accessible at `http://w3id.org/aiao`.
