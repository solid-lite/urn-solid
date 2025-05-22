## **Proposal: Provisional Solid Predicate Namespacing with `urn:solid:`**

### **Abstract**

This specification defines a lightweight convention for using the `urn:solid:` namespace for temporary RDF predicates within the Solid ecosystem. These predicates serve as stand-ins for future stable URIs and support early experimentation, mirroring the way developers use JSON keys before formalizing vocabularies.

---

### **1. Motivation**

Solid developers often need to introduce new predicates rapidly. Minting stable HTTP URIs can be too heavyweight during early stages. As in JSON development, temporary identifiers like `"foo"` or `"isCool"` are useful. In RDF, this role can be filled by simple URNs like:

```
urn:solid:isCool
```

This mirrors JSON idioms while staying valid within RDF and Linked Data tooling.

---

### **2. Syntax**

A `urn:solid:` predicate consists of:

```
urn:solid:{term}
```

Where `{term}`:

* Is a URI-safe string (letters, numbers, dashes, underscores)
* Follows lowercase or camelCase naming conventions
* Has no global coordination requirement

**Examples:**

* `urn:solid:syncToken`
* `urn:solid:hasFavoriteColor`
* `urn:solid:completedStep`

---

### **3. Usage Guidelines**

* Use `urn:solid:` predicates **only during prototyping**.
* Each predicate SHOULD be documented in context (e.g., app spec, data schema, README).
* Developers SHOULD prepare to **replace them** with persistent HTTP URIs as the project matures.

---

### **4. Trade-offs**

| Benefit                              | Cost                                          |
| ------------------------------------ | --------------------------------------------- |
| Simple and readable (like JSON keys) | Not globally unique                           |
| No need to mint HTTP URIs early      | Not dereferenceable                           |
| Easy to adopt in Solid apps          | Risk of name collisions if shared across apps |
| Encourages rapid development         | No discovery or linking via RDF tooling       |

---

### **5. Best Practices**

* Use unique, descriptive terms to avoid accidental reuse (`urn:solid:userPrefsTheme`)
* If a predicate is widely reused, promote it to a real HTTP URI
* Maintain a mapping table when upgrading:

  ```
  urn:solid:userPrefsTheme → https://example.com/ns#theme
  ```

---

### **6. Migration Path**

When predicates stabilize:

1. Mint a proper HTTP-based IRI, e.g., `https://example.org/ns#foo`
2. Map with `owl:equivalentProperty` or `rdfs:seeAlso`
3. Deprecate the `urn:solid:` form
4. Update apps and tooling incrementally

---

### **7. Conformance and Tooling**

* RDF processors MUST accept `urn:solid:*` as valid IRIs.
* Tools MAY warn or highlight their temporary status.
* Servers and apps SHOULD NOT depend on these as permanent identifiers.

---

### **8. Notes on URN Design**

While `urn:solid:` is not an IANA-registered namespace, its use in Solid is pragmatic and scoped. It aligns with [RFC 8141 §3.1](https://datatracker.ietf.org/doc/html/rfc8141#section-3.1) which allows informal URN namespaces for limited/private use.

---

### **9. Summary**

This proposal introduces a simple, JSON-inspired convention—`urn:solid:foo`—to allow RDF predicates to be created without ceremony. It supports developer agility, fosters rapid prototyping, and remains compatible with RDF tools while keeping a clear migration path to stable vocabularies.

