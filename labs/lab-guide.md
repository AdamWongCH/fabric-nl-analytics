# Lab Guide — Build & Debug a Data Agent

This lab is designed for **learning by doing**. 

- You will build an ontology and data agent  
- You will encounter errors  
- You will debug them  

You are expected to need about 1 hour (or more) to complete the lab.

---

# Goal

By the end of this lab, you will have:

- a semantic model  
- an ontology  
- a data agent  
- working queries  

---

# How to Approach This Lab

You are not just following steps.

Think in layers:

- **Keys** → define identity  
- **Relationships** → define structure  
- **Binding** → connects to data  
- **Instructions** → control behaviour

---

# Lab 1 — Load Data into Fabric

## Steps

1. Create a **Lakehouse**
   
<img width="259" height="149" alt="image" src="https://github.com/user-attachments/assets/105a0fce-88c0-45fb-bf1e-997ba42e381c" />

2. Upload the CSV files from `/data/sample/`

<img width="438" height="128" alt="image" src="https://github.com/user-attachments/assets/557ede38-3dc2-478b-9132-d3d7f90e99b5" />

3. Convert each CSV into a **table**

<img width="281" height="287" alt="image" src="https://github.com/user-attachments/assets/d803e33c-def0-44fa-973b-e400673e77f6" />

---

## Tables to create

- fact_resale_transaction  
- dim_location  
- dim_date  
- dim_flat  
- dim_lease  

---

## Screenshot
Lakehouse showing all 5 tables loaded

<img width="598" height="272" alt="image" src="https://github.com/user-attachments/assets/c7506eb8-268d-4d1c-8d4d-89a42caaec5b" />

## Why this matters

This is your **data foundation**.  
Everything else depends on clean and structured tables.

---

# Lab 2 — Create Semantic Model

## Steps

1. Create a **semantic model** from the lakehouse

<img width="594" height="349" alt="image" src="https://github.com/user-attachments/assets/bab0c61f-02d9-45a6-ab94-3dd13964721a" />


2. Include 3 tables
- fact_resale_transaction  
- dim_location  
- dim_date  

---

## Check

- `resale_price` shows ∑ (numeric)  
- tables are visible  

---

## Screenshot
Semantic model with tables

<img width="398" height="152" alt="image" src="https://github.com/user-attachments/assets/c1b4119e-2928-4097-83e2-4f264297acbe" />

---

## Why this matters

The semantic model defines:
- data types  
- relationships  
- aggregation behaviour  

This is what the ontology builds on

---

# Lab 3 — Define Relationships (Sementic Model)

## Create relationships

- fact_resale_transaction → dim_location (location_key)  
- fact_resale_transaction → dim_date (date_key)   

---

## Important

- Direction: fact → dimension  
- Cardinality: many-to-one  

---

## Screenshot
Model view with relationships

<img width="844" height="494" alt="image" src="https://github.com/user-attachments/assets/967c5f40-8dad-4e13-8615-24cc4f3ba6ed" />

---

## Why this matters

This defines how tables connect:
- fact → dimension  
- enables filtering and aggregation

---

# Lab 4 — Generate Ontology

## Steps

1. Generate ontology from semantic model

<img width="808" height="75" alt="image" src="https://github.com/user-attachments/assets/6d95e26a-ccb2-4b54-96e3-932fd07ef717" />

2. Rename entities:

| Table | Entity |
|------|--------|
| fact_resale_transaction | ResaleTransaction |
| dim_location | Location |
| dim_date | SaleMonth |

---

## Screenshot
Ontology entity list

<img width="863" height="434" alt="image" src="https://github.com/user-attachments/assets/64d89439-7dbc-434e-ae74-ec35288b9731" />

---

## Why this matters

Ontology translates:
- technical tables → business concepts  

This is what the agent understands

---

# Lab 5 — Define Keys

Each entity must have a key that uniquely identifies it.

---

## Steps

1. Open each entity  
2. Identify the key property  
3. Mark it as the key  

---

## Example

ResaleTransaction:
- transaction_id → key  

Location:
- location_key → key  

SaleMonth:
- date_key → key  

---

## Screenshot
Entity properties showing key selection

<img width="494" height="323" alt="image" src="https://github.com/user-attachments/assets/c5f3464e-c97a-4d5d-bfd6-bb1f8b81418a" />

---

## Why this matters

Keys define **identity**.

Without keys:
- relationships break  
- joins produce incorrect results  
- the agent may generate invalid queries  

---

# Lab 6 — Define Relationships (Ontology)

Relationships depend on correctly defined keys.

## Create

ResaleTransaction → located_at → Location  
ResaleTransaction → sold_in → SaleMonth  

---

## Check

- names read naturally  
- mapping is correct  

---

## Screenshot
Ontology relationship configuration

<img width="995" height="550" alt="image" src="https://github.com/user-attachments/assets/f53439b7-4e66-4571-944f-e01de88f6ae4" />

---

## Why this matters

Relationships define **meaning**:

- how entities connect  
- how questions are interpreted

---

# Lab 7 — Bind Data to Ontology

Binding links ontology properties to actual data columns.

---

## Two types of binding

### 1. Property binding
Example:
- resale_price → fact_resale_transaction.resale_price  

### 2. Relationship binding
Example:
- location_key = location_key  

---

## Steps

1. Open each entity  
2. Verify properties are mapped correctly  
3. Ensure relationship keys align

---

## Screenshot
Property binding screen

<img width="335" height="308" alt="image" src="https://github.com/user-attachments/assets/9698fbc0-8f9f-44b5-bdb3-fba81ece654f" />

---

## Why this matters

Without binding:
- the agent cannot access real data  
- queries may fail or return incorrect results  

If your agent does not work, check binding first

---

# Lab 8 — Create Data Agent

## Steps

1. Create a new Data Agent  
2. Attach the ontology  

---

## Screenshot
Data agent setup screen

<img width="331" height="151" alt="image" src="https://github.com/user-attachments/assets/86e02b76-3a27-4f9d-8dbc-ee136bf94121" />

<img width="310" height="248" alt="image" src="https://github.com/user-attachments/assets/5c36afa7-2cbe-4d01-83d7-2ead6aaaff46" />

---

## Why this matters

The agent is the interface between:
- user questions  
- data  

---

# Lab 9 — Add Agent Instructions

Copy and paste:

You are a data agent that answers questions about Singapore housing resale transactions.

Keep answers short, sharp, and concise.
Use only data from HousingResaleOntology.
Do not use outside knowledge.
Support group by in GQL.

Entities:
- ResaleTransaction = one HDB resale transaction
- SaleMonth = transaction month and year
- Location = region, town, street, block

Mappings:
- resale price, price, cost -> resale_price
- price per sqm -> price_per_sqm
- town -> Location.town
- region -> Location.region
- month -> SaleMonth.month_name
- year -> SaleMonth.sale_year

Rules:
- Normalize town names to the stored format when needed.
- For average price questions, return AVG(resale_price).
- For count questions, return COUNT(ResaleTransaction).
- For grouped summaries, return only grouped dimensions and aggregated measures.
- For detail questions, return row-level identifiers and measures without aggregation.
- Prefer business wording over technical column names.
- Mention SGD for prices and sqm for area.

---

## Why this matters

Instructions control:
- how the agent interprets questions  
- how it constructs queries
  
---

# Lab 10 — Try Basic Queries

Try:

- average resale price in BEDOK  
- which town has the highest number of transactions  
- compare Bedok and Tampines, which town is cheaper  

---

## Expected

- queries should work  
- results make sense  

---

# Lab 11 — Break It (Important)

Try:

> average resale price in BEDOK in January

---

## Expected

You may see an error

---

## Screenshot
Error message (GROUP BY / query failure)

<img width="242" height="258" alt="image" src="https://github.com/user-attachments/assets/b3f62fa8-ce0b-48e5-8bcd-7d211c746afb" />
  
---

## Why this matters

This tests:
- multi-filter logic  
- aggregation behaviour

This causes invalid queries

---

# Lab 12 — Debug It

## Step 1 — Think like the agent

Break it down:

- Entity → ResaleTransaction  
- Filters → town + month  
- Operation → average  

---

## Step 2 — Identify problem

Common issue:
- mixing detail + summary  

Error:
transaction_id not in GROUP BY  

---

## Step 3 — Fix instructions

Add in Rules:
For aggregated queries, do not return transaction_id or any row-level identifier.


---

# Lab 13 — Try Again

Retry:

> average resale price in BEDOK in January  

---

## Expected

Improved or working result  

---

# Reflection

Think about:

- What worked?  
- What failed?  
- Why did it fail?  
- What did you change?  

---

# Optional Challenge 1

Try more queries:
 
- highest price town  
- monthly trend for BEDOK  

Are the answers correct? If not, how would you resolve?

---

# Optional Challenge 2

Try enriching the ontology:
 
- Adding a new Flat entity
- Defining Flat key
- Creating relationship between Flat and ResaleTransaction
- Binding Flat properties
- Repeating Lab 4 to Lab 13

<img width="994" height="473" alt="image" src="https://github.com/user-attachments/assets/2a7f4fc0-ab42-4023-b0fd-2165b5b323f3" />

---

# Key Learning

- The agent does not think for you  
- You must structure the problem correctly  

---

# Notes

- Fabric IQ is in preview  
- Some behaviours may be inconsistent  
- Debugging is part of the learning process  
