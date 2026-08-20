# Product details (product)

## Core features

- JSON-based agent definitions with persona, expertise, and knowledge tags
- spawn placement based on zones and tags
- handoffs between agents
- local knowledge base with keyword search

## Typical use cases

- trade-show demonstrations with sales, technical, and marketing experts
- virtual showrooms with several interaction stations
- onboarding kiosks for internal tools

## Limitations

- no vector database; retrieval uses a small deterministic keyword search
- language-model responses require valid local model credentials

## Technical requirements

- Python 3.10 or newer
- Unity WebRequest or another HTTP client in the frontend

