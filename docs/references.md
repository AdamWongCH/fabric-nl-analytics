# References

This page lists official and supporting references for the concepts used in this learning repo.

The main learning materials are intentionally written in simple language.  
Use this page if you want to read the source documentation in more depth.

---

## Microsoft Fabric IQ and Ontology

### Microsoft Fabric IQ documentation

Microsoft Fabric IQ introduces ontology and graph-based capabilities for organising data around business entities, relationships, and logic.

- [Fabric IQ documentation](https://learn.microsoft.com/en-us/fabric/iq/)

---

### Ontology in Microsoft Fabric IQ

Ontology provides a shared business model that defines entities, properties, relationships, and bindings.

- [What is ontology in Fabric IQ](https://learn.microsoft.com/en-us/fabric/iq/ontology/overview)

---

### Fabric IQ ontology tutorials

These tutorials walk through creating, enriching, previewing, and using an ontology with a data agent.

- [Tutorial 0 — Introduction and environment setup](https://learn.microsoft.com/en-us/fabric/iq/ontology/tutorial-0-introduction)
- [Tutorial 1 — Create ontology](https://learn.microsoft.com/en-us/fabric/iq/ontology/tutorial-1-create-ontology)
- [Tutorial 2 — Enrich ontology](https://learn.microsoft.com/en-us/fabric/iq/ontology/tutorial-2-enrich-ontology)
- [Tutorial 3 — Preview ontology](https://learn.microsoft.com/en-us/fabric/iq/ontology/tutorial-3-preview-ontology)
- [Tutorial 4 — Create data agent](https://learn.microsoft.com/en-us/fabric/iq/ontology/tutorial-4-create-data-agent)

---

## Microsoft Fabric Data Agent

### Fabric data agent concept documentation

Fabric data agents allow users to ask questions about data using natural language. They use context from selected data sources to generate, validate, execute, and format responses.

- [Fabric data agent concept documentation](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)

---

### Create a Fabric data agent

This guide explains how to create a Fabric data agent in a workspace and connect it to data sources.

- [Create a Fabric data agent](https://learn.microsoft.com/en-us/fabric/data-science/how-to-create-data-agent)

---

### Fabric data agent end-to-end tutorial

This tutorial shows how to create and use a Fabric data agent with a lakehouse.

- [Fabric data agent scenario tutorial](https://learn.microsoft.com/en-us/fabric/data-science/data-agent-end-to-end-tutorial)

---

## Graph and GQL

### Graph in Microsoft Fabric

Microsoft Fabric Graph supports connected data analysis using graph structures and GQL.

- [Graph in Microsoft Fabric overview](https://learn.microsoft.com/en-us/fabric/graph/overview)

---

### GQL Language Guide

GQL is the graph query language used to query graph data using patterns.

- [GQL Language Guide for graph in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/graph/gql-language-guide)

---

### GQL Quick Reference

A concise reference for common GQL syntax.

- [GQL Quick Reference for graph in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/graph/gql-reference-abridged)

---

### GQL Graph Patterns

Graph patterns are used to describe connected structures in GQL queries.

- [GQL Graph Patterns for graph in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/graph/gql-graph-patterns)

---

### GQL Values and Value Types

This reference explains values, types, and why type handling matters in GQL queries.

- [GQL Values and Value Types for graph in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/graph/gql-values-and-value-types)

---

## Standards

### ISO/IEC 39075:2024 — GQL

ISO/IEC 39075:2024 defines data structures and operations for property graphs and specifies the syntax and semantics of GQL.

- [ISO/IEC 39075:2024 — Database languages — GQL](https://www.iso.org/standard/76120.html)

---

## NL2GQL and Text-to-Graph Query Generation

NL2GQL refers to the task of translating natural language questions into graph query language.  
In this repo, NL2GQL is used as an advanced learning concept rather than as a claim that Fabric currently exposes a dedicated “NL2GQL” feature by that name.

The practical idea is that a natural-language graph question usually requires several steps:

1. Understanding the user’s intent.
2. Identifying relevant entities and relationships.
3. Aligning the question with the graph schema or ontology.
4. Generating a valid graph query.
5. Checking, refining, and explaining the result.

These references are useful for the advanced track, especially for understanding how natural language questions can be translated into graph queries.

- Zhou, Y. et al. (2024). **R³-NL2GQL: A Model Coordination and Knowledge Graph Alignment Approach for NL2GQL**. *Findings of EMNLP 2024*.  
  Useful for understanding ranking, rewriting, refinement, and schema alignment in NL2GQL.  
  Link: https://aclanthology.org/2024.findings-emnlp.800/

- Liang, Y. et al. (2024). **NAT-NL2GQL: A Novel Multi-Agent Framework for Translating Natural Language to Graph Query Language**. *arXiv*.  
  Useful for understanding multi-agent NL2GQL pipelines with preprocessing, generation, and refinement.  
  Link: https://arxiv.org/abs/2412.10434

- Ozsoy, M. G. et al. (2025). **Text2Cypher: Bridging Natural Language and Graph Databases**.  
  Useful as a related Text-to-Cypher reference for graph query generation.  
  Link: https://aclanthology.org/2025.genaik-1.11/

- Ni, P. et al. (2024). **Knowledge Graph and Deep Learning-based Text-to-GQL Model for Intelligent Medical Consultation Chatbot**. *Information Systems Frontiers*.  
  Useful for domain-specific Text-to-GQL and chatbot integration.  
  Link: https://link.springer.com/article/10.1007/s10796-022-10295-0

- Jia, Y. et al. (2025). **Text-to-graph query using semantic subgraph retrieval**. *Knowledge-Based Systems*.  
  Useful for retrieval-augmented text-to-graph query generation.  
  Link: https://www.sciencedirect.com/science/article/abs/pii/S0950705125022452

---

## How These References Relate to This Repo

This repo simplifies the above materials into a guided learning path.  
It does not attempt to reproduce the full official documentation or research literature.  
Instead, it uses them to support a practical learning progression from ontology design to graph querying and natural-language querying.

| Repo concept | Related source | How it is used in this repo |
|---|---|---|
| Ontology | Fabric IQ ontology documentation | Used to explain how business entities, properties, relationships, and bindings create a shared semantic layer. |
| Data agent | Fabric data agent documentation | Used to explain how users can ask questions about data in natural language without writing SQL, DAX, KQL, or graph queries directly. |
| Graph concepts | Fabric Graph documentation | Used to introduce connected data, nodes, relationships, and graph-based analysis. |
| GQL | Fabric GQL Language Guide and ISO/IEC 39075 | Used to teach the basic query patterns needed to retrieve and reason over connected data. |
| NL2GQL | NL2GQL and text-to-graph-query research papers | Used in the advanced track to explain how natural language questions may be translated into graph queries through schema understanding, query generation, validation, and refinement. |
| Query examples and evaluation | Fabric data agent examples and evaluation documentation | Used to show why example questions, expected outputs, testing, and evaluation are important when building reliable natural-language data experiences. |
| Debugging query failures | GQL documentation, data agent documentation, and NL2GQL research | Used to explain common failure points such as unclear user questions, weak schema descriptions, missing relationships, incorrect filters, invalid query patterns, and unsupported assumptions. |

---

## Core Message

The central idea of this repo is:

> Structure enables AI to reason over data more reliably.

In this repo, “structure” refers to ontology, graph relationships, clear data descriptions, governed examples, and query patterns. These structures help reduce ambiguity when users ask questions in natural language.
