---
name: toon
description: TOON (Token-Oriented Object Notation) syntax reference — a compact, lossless encoding of the JSON data model for LLM prompts. Load when producing or reading TOON and you need the exact syntax: the four forms, nesting, guardrails, and quoting.
---

# TOON syntax

TOON encodes the JSON data model in fewer tokens using YAML-style indentation plus CSV-style tables. It is lossless — keep data as JSON in code, encode to TOON for prompts. Its sweet spot is **uniform objects** (same fields across items). For deeply nested or non-uniform data, JSON may be smaller — use judgment.

## Objects and primitives

Two-space indentation instead of braces; `key: value` per line:

```
location:
  city: Berlin
  country: DE
  units: metric
```

## The four array/map forms

Pick the form from the data's shape.

**1. Inline** — a primitive array on its header line. `[N]` is the length:
```
alerts[2]: frost,wind
tags[0]:
```

**2. Tabular** — a uniform array of objects. Declare the fields once in `{}`, then one row per element, values comma-separated in field order:
```
forecast[3]{day,condition,rainChance}:
  Mon,snow,80
  Tue,cloudy,20
  Wed,sunny,5
```
Uniform nested objects fold into the header as a **nested field group**; rows stay flat:
```
forecast[2]{day,temp{min,max},condition}:
  Mon,-2,4,snow
  Tue,1,7,cloudy
```

**3. Keyed tabular** — a map whose values are uniform objects (records by ID, config, flags). Mark it with a colon after the length, `[N:]`; each row carries its own key:
```
environments[2:]{region,replicas,debug}:
  production: eu-central-1,6,false
  staging: eu-central-1,2,true
```

**4. List** — the fallback for mixed types or non-uniform objects. One `- ` per element; a bare `-` is an empty object:
```
items[2]:
  - id: 1
    name: Alice
  - id: 2
    tags[2]: a,b
```

## Guardrails

`[N]` declares how many rows and `{fields}` how many columns, so a truncated or malformed table is detectable. Keep row and field counts exact.

## Quoting

Quote a string only when needed — it contains the delimiter (comma), a colon, leading/trailing whitespace, or would otherwise read as something else (a number, `true`/`false`, `null`). Otherwise leave it bare.
