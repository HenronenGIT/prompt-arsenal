## Red Flags — AVOID These at All Costs

When generating or modifying code, actively check for and eliminate these anti-patterns:

### 🚩 1. Shallow Module
**What it looks like:** The interface of a class or method is nearly as complex as its implementation.  
**What to do instead:** Combine shallow modules into deeper ones. A class with a single trivial method or a thin wrapper that adds no real value is a shallow module. Prefer fewer, deeper modules over many small, shallow ones. Resist "classitis" — the urge to create a class for everything.

### 🚩 2. Information Leakage
**What it looks like:** The same design decision (data format, protocol detail, algorithm choice) is reflected in multiple modules.  
**What to do instead:** Encapsulate shared knowledge in a single module. If two classes both know about a file format, merge them or extract a shared module that owns that knowledge.

### 🚩 3. Temporal Decomposition
**What it looks like:** Code is structured around the order operations happen (read → parse → process → write) rather than around information hiding.  
**What to do instead:** Structure modules around the information they encapsulate, not the sequence of execution. Operations that share knowledge should live together regardless of when they execute.

### 🚩 4. Overexposure
**What it looks like:** An API forces callers to be aware of rarely used features just to perform common operations.  
**What to do instead:** Design the API so the common path is simple. Use sensible defaults. Hide edge cases behind optional parameters or separate methods.

### 🚩 5. Pass-Through Method
**What it looks like:** A method does almost nothing except forward its arguments to another method with a similar or identical signature.  
**What to do instead:** Expose the lower-level class directly, redistribute functionality so each method adds real value, or merge classes. Decorators and wrappers are frequent offenders — use them sparingly.

### 🚩 6. Repetition
**What it looks like:** The same non-trivial code pattern appears in multiple places.  
**What to do instead:** Extract the repeated logic into a shared utility, method, or module. If code snippets are evolving at the same rate for the same reason, they are true duplication and must be unified.

### 🚩 7. Special-General Mixture
**What it looks like:** Special-purpose (application-specific) code is tangled with general-purpose (reusable) code in the same module or method.  
**What to do instead:** Cleanly separate them. The general-purpose mechanism should know nothing about its specific callers. Special-purpose code should live in a higher layer.

### 🚩 8. Conjoined Methods
**What it looks like:** Two methods are so intertwined that you can't understand one without reading the other.  
**What to do instead:** Refactor to make each method independently understandable. Reduce shared mutable state. Clarify interfaces between them. Consider merging if separation is artificial.

### 🚩 9. Comment Repeats Code
**What it looks like:** A comment restates what the code already says (e.g., `// increment i` above `i++`).  
**What to do instead:** Write comments that add value — explain *why*, describe non-obvious behavior, define abstractions, note boundary conditions, or specify units and invariants. Use different words than the code uses.

### 🚩 10. Implementation Documentation Contaminates Interface
**What it looks like:** An interface comment (JSDoc, docstring) describes internal implementation details that callers don't need to know.  
**What to do instead:** Interface documentation should describe *what* the module does for the caller: behavior, parameters, return values, side effects, and constraints. Keep implementation details in implementation comments only.

### 🚩 11. Vague Name
**What it looks like:** A variable or method name is so generic it conveys almost no useful information (e.g., `data`, `result`, `temp`, `handle`, `process`, `info`).  
**What to do instead:** Choose names that create a clear mental image of what the entity is and what it is *not*. Be precise. Use predicates for booleans (`isVisible`, `hasPermission`). Every word in a name should add information.

### 🚩 12. Hard to Pick Name
**What it looks like:** You struggle to find a precise and intuitive name for an entity.  
**What to do instead:** This is a signal that the entity's purpose is unclear or its design is muddled. Reconsider the design. A well-designed entity is easy to name.

### 🚩 13. Hard to Describe
**What it looks like:** Documenting a variable or method requires a long, complex explanation.  
**What to do instead:** This signals a design problem — the entity is doing too much or its abstraction is poor. Simplify or decompose it until it can be described concisely. If a comment must be long, the interface is too complex.

### 🚩 14. Nonobvious Code
**What it looks like:** A reader cannot quickly understand what a piece of code does or why it behaves a certain way.  
**What to do instead:** Restructure the code for clarity. Use better names, add whitespace, break complex expressions into named intermediate variables, and add comments explaining the non-obvious parts. Optimize for reading speed, not writing speed.

---

## Coding Practices to Enforce

- **Avoid configuration parameter sprawl** — compute reasonable defaults internally instead of exposing every decision to the caller.
- **Handle exceptions at the lowest level possible** — mask, aggregate, or define errors out of existence rather than propagating them upward.
- **Don't split methods just because they're long** — length alone is not a problem. Split only when a method has an overly complex interface or does genuinely unrelated things. A long, readable method with a simple signature is better than many short, shallow methods.
