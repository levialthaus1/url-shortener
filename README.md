# URL Shortener Simple Project

The aim of this project is to take in a long URL and return a shortend URL that links to the original.

# Architecture
    ┌──────────────────────┐
    │       Browser        │
    │  public/index.html   │
    │  (form + fetch())    │
    └──────────┬───────────┘
               │   HTTP
               ▼
    ┌──────────────────────┐
    │    Express server    │
    │      server.js       │
    │                      │
    │  POST /api/links     │
    │  GET  /:code         │
    └──────────┬───────────┘
               │ SQL
               ▼
    ┌──────────────────────┐
    │   SQLite (links.db)  │
    │  links: id, code,    │
    │  url, clicks,        │
    │  created_at          │
    └──────────────────────┘

User submits form
  → fetch POST /api/links  { url: "https://..." }
  → server validates URL, generates code "abc123"
  → INSERT INTO links
  → 201 Created  { code: "abc123", shortUrl: "http://localhost:3000/abc123" }
  → page shows the short link

Browser goes to /abc123
  → server: SELECT url FROM links WHERE code = 'abc123'
  → found:     clicks + 1, then 302 redirect → original URL
  → not found: 404