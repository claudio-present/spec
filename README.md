# spec

A contract is a formal, machine-checkable definition of an interface,
independent of whatever implements or calls it — as opposed to a prose spec,
which describes intent but isn't specific enough to generate code from or
verify automatically. Every interface here today is an API, but the folder
isn't scoped to APIs specifically — a module-level contract fits the same
structure.

```
contracts/
├── owned/      # interfaces this spec owns — the contract is authoritative
└── consumed/   # interfaces this spec only consumes — pinned expectations

edgecases/      # numbered boundary conditions and failure modes
```

See [contracts/README.md](contracts/README.md) and
[edgecases/README.md](edgecases/README.md) for why each exists.
