# Data Models

## 1. Relational Model

Data is organized into **tables** (rows & columns) with relationships between them. A strict schema is enforced.

> **Example databases:** PostgreSQL, MySQL

**Example — the same user data split across tables:**

`users` table:
| id | name  | email             |
|----|-------|-------------------|
| 1  | Alice | alice@example.com |

`addresses` table:
| id | user_id | type | city     |
|----|---------|------|----------|
| 1  | 1       | home | New York |
| 2  | 1       | work | Boston   |

`orders` table:
| id | user_id | item   | price |
|----|---------|--------|-------|
| 1  | 1       | Laptop | 1200  |
| 2  | 1       | Mouse  | 25    |

To get all data for Alice, you'd **join** all three tables using `user_id`.

---

### Normalization

Store data **once**, reference it everywhere by ID.

**Benefits:** No duplication, no typos, easy to update.

**When to give something an ID:**
- The value repeats across multiple rows
- The value can change over time
- It's one of a fixed set of options (e.g. countries, categories)

**When you don't need an ID:**
- The value is unique to that row and will never be reused

---

### Relationship Types

| Type | Description | Example |
|------|-------------|---------|
| **One-to-One** | One record in A → exactly one in B | One user has one passport |
| **One-to-Many** | One record in A → many in B | One user has many orders |
| **Many-to-One** | Many in A → one in B | Many users live in one city |
| **Many-to-Many** | Many in A ↔ many in B | Students and courses |

**Many-to-Many** requires a **junction table** (also called a join or bridge table) — a middle table that splits the relationship into two one-to-many relationships.

---

## 2. Document Model

Instead of splitting data across multiple tables, everything related is stored **together in one document** (usually JSON format).

Think of it like a folder — instead of filing each piece of information separately, you keep everything about one "thing" in a single place.

**Example — the same user data as one document:**
```json
{
  "name": "Alice",
  "email": "alice@example.com",
  "addresses": [
    { "type": "home", "city": "New York" },
    { "type": "work", "city": "Boston" }
  ],
  "orders": [
    { "item": "Laptop", "price": 1200 },
    { "item": "Mouse", "price": 25 }
  ]
}
```

No joins needed — everything about Alice is in one place, loaded in one read.

**Best for:** Data that naturally belongs together and is always read together.

**Not great for:** Data with complex relationships across many different entities (joins get messy).

> **Example databases:** MongoDB, CouchDB
