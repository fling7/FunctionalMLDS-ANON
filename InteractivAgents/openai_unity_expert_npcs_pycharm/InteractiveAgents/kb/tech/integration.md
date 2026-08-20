# Technology and integration (tech)

## Architecture

- Python HTTP server built with the standard library
- Unity client using the `/setup` and `/chat` REST endpoints
- structured JSON responses validated against schemas

## Integrations

- Unity through WebRequest or HttpClient
- external systems through a JSON bridge

## Operation and security

- no database setup required
- API credentials supplied through an ignored `config.json` or the
  `OPENAI_API_KEY` environment variable
- CORS support for WebGL clients

## Performance notes

- keyword retrieval suited to small and medium knowledge collections
- multiple agents supported in one session
