# AIAO: Anthropogenic Impact Accounting Ontology

## Overview

The Anthropogenic Impact Accounting Ontology (AIAO) provides a semantic framework for accounting for human impact on environments. This ontology is the product of collaboration by members of the Standards Working Group of the LFDT CA2SIG. It is one of four ontologies.

## Linked Ontologies

AIAO is closely integrated with three complementary ontologies:

- **[ImpactOnt](http://w3id.org/impactont)**: Provides foundational concepts for impact modeling, including Events, Processes, and Things

- **[Claimont](http://w3id.org/claimont)**: Defines the structure for claims and assertions about impacts
  
- **[InfoComm](http://w3id.org/infocomm)**: Handles information and communication aspects related to impact reporting

Together, these four ontologies comprise a suite of ontologies for environmental and social impact accounting.

## Core Classes

### Agent (`aiao:Agent`)

Represents any entity that bears accountability for the occurrence of activities. This includes:

- Natural persons
- Legal persons (companies, organizations)
- Cyber personas (digital identities)

Agents may comprise other agents, allowing for complex organizational structures.

### Activity (`aiao:Activity`)

An event orchestrated by an agent that impacts an environment. Activities are the primary units of impact generation.

### Environment (`aiao:Environment`)

The surroundings and conditions in which a thing occur. This encompasses, inter alia:

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

- **ImpactClaim (`aiao:ImpactClaim`)**: Claims about causal relations between events and state differences

- **StateClaim (`aiao:StateClaim`)**: Claims about the state of things

## Key Relationships

### Agent-Activity Relations

The ontology models complex relationships between agents and activities through:

- `hasAgentActivityRelation`: Links activities to their agent relationships
- `hasAgent`: Identifies the agent in a relationship
- `hasActivity`: Identifies the activity in a relationship
- `governs`: Describes how roles control agent-activity relationships

### Activity Controls

Activities are governed by various controls:

- `hasObjective`: Links activities and projects to their goals
- `isPerformedWith`: Connects activities to their instruments
- `comprisesActivity`: Shows how projects contain multiple activities

## Usage

AIAO is particularly suited for:

- Environmental impact assessment
- Corporate sustainability reporting
- Carbon accounting and offset projects
- Social impact measurement
- Regulatory compliance tracking

## Namespace

The ontology uses the namespace `http://w3id.org/aiao#` and is accessible at `http://w3id.org/aiao`.
