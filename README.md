# Natural Language Analytics with Fabric IQ

Build your first **Ontology** and **Data Agent** using Microsoft Fabric.

This repository is a **hands-on learning experience** to help you understand how data agents work, and how to build, test, debug, and improve them.

---

## What This Is About

This is not just about using a tool.

It is about understanding how **structure enables AI to reason over data**.

You will learn how data is transformed from raw tables into meaningful structures that allow AI systems to interpret questions, generate queries, and return useful answers.

---

## Learning Outcomes

By the end of the beginner track, you should be able to:

- explain what an ontology is in simple terms
- describe how data agents use structured data to answer questions
- map a natural language question into entities, filters, and operations
- build a basic ontology and data agent in Microsoft Fabric
- debug common issues such as missing mappings, case sensitivity, and GROUP BY errors

By the end of the advanced track, you should be able to:

- explain how natural language is translated into GQL
- design better ontology structures for data-agent reasoning
- write stronger data-agent instructions
- troubleshoot failed generated queries
- evaluate data-agent behaviour using test questions

---

## Beginner Track — Start Here

Estimated time: **1 hour 20 minutes**

Suggested flow:

- 5 min — graph concepts
- 10 min — learning guide
- 1 hour — hands-on lab
- 5 min — reflection

Follow this sequence:

1. **Understand basic graph concepts** *(optional but helpful)*  
   [`/docs/graph-basics.md`](./docs/graph-basics.md)

2. **Understand how the system works**  
   [`/docs/learning-guide.md`](./docs/learning-guide.md)

3. **Refer to key terms when needed**  
   [`/docs/glossary.md`](./docs/glossary.md)

4. **Build and try it yourself**  
   [`/labs/lab-guide.md`](./labs/lab-guide.md)

5. **Consolidate your understanding**  
   [`/docs/summary.md`](./docs/summary.md)

6. **Read further references** *(optional)*  
   [`/docs/references.md`](./docs/references.md)

---

## Advanced Track

After completing the beginner track, continue with the advanced materials.

Estimated time: **2–3 hours**

The advanced track focuses on how to **design, evaluate, debug, and improve ontology-powered data agents**.

Follow this sequence:

1. **Advanced NL2GQL**  
   Learn how natural language questions are translated into graph queries.  
   [`/docs/advanced-nl2gql.md`](./docs/advanced-nl2gql.md)

2. **Ontology Design Principles**  
   Learn how entity names, relationships, keys, and bindings affect agent behaviour.  
   [`/docs/ontology-design-principles.md`](./docs/ontology-design-principles.md)

3. **Agent Instruction Design**  
   Learn how to write stronger instructions for mapping, normalization, aggregation, and error prevention.  
   [`/docs/agent-instruction-design.md`](./docs/agent-instruction-design.md)

4. **Troubleshooting Guide**  
   Learn how to diagnose common agent failures such as case mismatch, missing relationship paths, and GROUP BY errors.  
   [`/docs/troubleshooting-guide.md`](./docs/troubleshooting-guide.md)

5. **Evaluation Framework**  
   Learn how to test whether the data agent is interpreting questions correctly.  
   [`/docs/evaluation-framework.md`](./docs/evaluation-framework.md)

---

## Advanced Labs

Use these labs after completing the beginner lab.

1. **Advanced NL2GQL Lab**  
   Practise breaking natural language questions into entity, filter, relationship path, operation, and expected GQL shape.  
   [`/labs/lab-advanced-nl2gql.md`](./labs/lab-advanced-nl2gql.md)

2. **Debugging Agent Errors Lab**  
   Practise reading failed queries and improving ontology or agent instructions.  
   [`/labs/lab-debugging-agent-errors.md`](./labs/lab-debugging-agent-errors.md)

3. **Agent Evaluation Lab**  
   Build a small benchmark of test questions and score the agent’s behaviour.  
   [`/labs/lab-agent-evaluation.md`](./labs/lab-agent-evaluation.md)

---

## What You Will Build

You will build a simple natural language analytics workflow consisting of:

- Semantic model
- Ontology
- Data agent

Together, these enable **self-service analytics using natural language**.

---

## Key Idea

```text
Data → Semantic Model → Ontology → Data Agent → Answers
