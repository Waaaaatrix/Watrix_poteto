---
name: principle-module-decoupling
description: "Apply when designing or changing modules or features within a system. Let them evolve independently through stable, reusable interfaces defined in a shared protocol layer."
disable-model-invocation: true
---

# Module Decoupling

Apply this between modules inside a system. Boundary Discipline covers data crossing into the system from outside.

- Define shared interfaces and types in a protocol layer. Make their inputs, outputs, and semantics explicit. Modules depend on that contract and keep their implementation details private.
- Keep the contract stable as implementations change. An internal change should stay within its module. If the contract must change, make the adjustments for callers small and explicit.
- When adding a module or capability, design its interface for other likely consumers. Expose the reusable capability without baking the first caller's workflow into the contract.

The test: can you change one implementation or add another caller through the contract without rewriting unrelated modules?
