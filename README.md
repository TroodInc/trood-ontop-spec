# trood-ontop-spec

Canonical JSON Schema specification for trood-ontop.

This repository defines and freezes the structural contracts that SHALL govern all trood-ontop runtime implementations.

The specification defines:

- Ontology model structure
- Dependency graph structure
- Rule DSL structure

All published schemas in this repository are normative.

---

# 1. Status

Version: v0.1 (Phase 1 Freeze)

This version defines the minimal deterministic contract REQUIRED to:

- Validate canonical dependency graph JSON
- Validate canonical rule DSL JSON
- Guarantee cross-runtime structural parity
- Support the golden conformance corpus maintained in the coordination repository

This version is considered frozen except for PATCH-level corrections.

---

# 2. Normative Artifacts

The directory:

```
schemas/
  ontology.schema.json
  graph.schema.json
  rules.schema.json
```

constitutes the ONLY normative outputs of this repository.

No other documents, examples, or commentary override these schema files.

In the event of ambiguity, schema definitions are authoritative.

---

# 3. Normative Language

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, and MAY are to be interpreted as described in RFC 2119.

---

# 4. Architectural Principles

## 4.1 Executable Specification

The specification SHALL be executable via JSON Schema validation.

All published schemas MUST validate:

- The normative seed graph
- The normative seed rule DSL
- The golden conformance corpus

If a schema and the conformance corpus disagree, the conformance corpus SHALL be considered authoritative and the schema MUST be corrected via versioned update.

---

## 4.2 Determinism

All conforming runtimes MUST:

- Accept JSON documents that validate against this specification
- Reject JSON documents that do not validate
- Produce deterministic validation results for identical input

Runtime behavior SHALL NOT depend on non-deterministic factors such as prompts, time, randomness, or external AI inference.

---

## 4.3 Canonical Format

In v0.1:

- Graph input SHALL be JSON
- Rule DSL SHALL be JSON
- Validation SHALL be defined exclusively via JSON Schema

No alternative DSL formats, encodings, or transport representations are canonical in v0.1.

---

# 5. Normative Seed Graph (Minimum Contract)

The following document MUST validate against `graph.schema.json`:

```json
{
  "version": "0.1",
  "nodes": [
    { "id": "svc.a", "kind": "service" },
    { "id": "svc.b", "kind": "service" }
  ],
  "edges": [
    { "from": "svc.a", "to": "svc.b", "kind": "depends_on" }
  ]
}
```

---

# 6. Normative Seed Rule DSL (Minimum Contract)

The minimal Rule DSL document MUST support the following structure and MUST validate against `rules.schema.json`:

```json
{
  "version": "0.1",
  "rules": [
    {
      "kind": "forbid_dependency",
      "from": "svc.a",
      "to": "svc.b"
    }
  ]
}
```

---

# 7. Versioning Policy

Semantic Versioning SHALL be used:

- MAJOR → Breaking schema changes
- MINOR → Backward-compatible structural extensions
- PATCH → Non-structural corrections or clarifications

If a schema change invalidates any existing conformance case:

- A MAJOR version bump is REQUIRED
- Migration notes MUST be provided
- The conformance corpus MUST be updated in coordination

No breaking change SHALL be introduced without version increment.

---

# 8. Repository Boundaries

Repository roles are strictly separated:

- trood-ontop → coordination and conformance corpus
- trood-ontop-validator-go → Go runtime implementation
- trood-ontop-validator-ts → TypeScript runtime implementation

This repository defines structure.
Runtime repositories define behavior.

Runtime repositories MUST NOT alter or redefine ontology structure.

---

# 9. Non-Goals (v0.1)

The following are explicitly out of scope for v0.1:

- Advanced graph semantics
- Extensibility frameworks
- Plugin mechanisms
- Execution engines
- Optimization layers

v0.1 freezes the minimal viable deterministic structural contract.

Future versions MAY extend capabilities but MUST preserve versioned compatibility guarantees.
