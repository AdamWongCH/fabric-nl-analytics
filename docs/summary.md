# Summary — What You Have Learned

---

## Big Picture

You started with raw data.

```text
Data → Semantic Model → Ontology → Data Agent → Answer
```

You transformed data into a structured layer that allows questions to be interpreted and answered.

This is the key idea:

> Structure enables AI to reason over data.

---

## What You Now Understand

You have learned how:

- data becomes **meaning** through ontology  
- meaning enables **queries** through GQL  
- queries produce **answers** through the data agent  
- structure determines whether the agent works correctly  

---

## Core Concepts

You should now be comfortable with:

| Concept | Meaning |
|--------|---------|
| Entity | what things are |
| Relationship | how things connect |
| Property | what we know about them |
| Key | identity |
| Binding | linking ontology to data |
| Instructions | guiding agent behaviour |

---

## What You Experienced

You likely encountered:

- queries that failed  
- results that did not make sense  
- errors such as GROUP BY issues  

These are not random failures.

They usually happen when **structure is incomplete, unclear, or incorrect**.

For example:

- missing keys can affect relationships  
- incorrect binding can affect query results  
- unclear instructions can affect generated queries  
- mixing row-level fields with aggregates can cause GROUP BY errors  

---

## Final Insight

You are not just using a tool.

You are designing how data is structured so that AI can reason over it.

Better structure leads to better answers.

---

## Reflection

Take a moment to think:

- What worked?  
- What failed?  
- Why did it fail?  
- How did structure affect the outcome?  
- What would you improve in the ontology or agent instructions?  

---

## What’s Next

Now that you understand the basics, you can:

- try more complex queries  
- refine your ontology  
- improve data agent instructions  
- apply this approach to real datasets  
- explore advanced topics such as NL2GQL, troubleshooting, and evaluation  

---

## Final Thought

If everything worked perfectly… you might want to break it again 😄

That is where the real learning happens.

---

## Further Reading

See the references page for official Microsoft and GQL documentation:

[`/docs/references.md`](./references.md)
