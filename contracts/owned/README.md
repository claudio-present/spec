# Owned contracts

Interfaces this spec owns and exposes to other services, clients, or
modules — APIs today, but the same folder covers any interface this spec
defines, network or not.

## What goes here

One contract file per owned interface (an OpenAPI document, a `.proto`
file, a GraphQL schema, a plain type/interface definition for a non-network
module — whatever format fits), named for the service or module it
describes.

## Why it comes first

For an owned interface, the contract is written *before* the implementation
and is authoritative — the implementation has to conform to it, not the
other way around. That ordering is what makes codegen, mocking, and
automated conformance checks possible; without it, docs and code drift
apart silently.

**Source:** [API-First Design](https://swagger.io/resources/articles/adopting-an-api-first-approach/)
(SmartBear/Swagger) — the practice this folder implements.
