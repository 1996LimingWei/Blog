## What is client/trip-frontend/src/types/index.ts
<img width="571" height="399" alt="截屏2025-07-28 13 15 59" src="https://github.com/user-attachments/assets/f1be228c-8697-4428-bd54-fa67dfbe7eed" />

`index.ts` is a **TypeScript type definitions file**. It's like a "dictionary" or "contract" that defines the shape and structure of data that your application uses.

It is like **blueprint** that tells TypeScript:
- "What properties should a `User` object have?"
- "What should a `Song` object look like?"
- "What parameters should this function accept?"

---

## What is `export` and what does it do?

`export` makes the subquent content (like a type, interface, or function) available to be used in other files, so other files can `import` it and use it.
TypeScript will check that the data matches the defined structure

---

Both `type` and `interface` are ways to define the shape of data in TypeScript. They tell TypeScript what properties an object should have and what types those properties should be.

---


## Summary

- **Interface**: For object shapes, extensible, works with classes
- **Type**: For unions, functions, complex combinations, utility types
- **Rule of thumb**: Start with `interface` for objects, use `type` when you need unions or functions
