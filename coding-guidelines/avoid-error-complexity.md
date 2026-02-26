# Do Not Spread Error Complexity

## Principle
If a method throws or returns `null/error`, every caller must handle it.  
Only expose errors when callers must make a real decision.

## Default
- Handle internal/technical failures inside the module.
- Expose only domain/business errors that affect caller behavior.

## API Guidance
- Avoid leaking low-level exceptions (I/O, parsing, network).
- Avoid `null/undefined` unless absence is expected and actionable.
- Prefer:
  - `getExistingUser(id): User`  // absence = bug
  - `findUser(id): User | null`  // absence = valid case

## Smell
If multiple callers repeat the same error handling → move it into the module.

## Goal
Absorb complexity inside modules. Keep interfaces simple and decisive.
