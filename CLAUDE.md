# grand-line

One Piece filler guide — filler.polpo.tech

## Stack

- HTML/CSS/JS statico (no framework, no dipendenze)
- Hosting: Cloudflare Pages
- Dominio: filler.polpo.tech

## Dev locale

```bash
npx serve
```

Apri http://localhost:3000

## Deploy

```bash
export PATH="$HOME/.nvm/versions/node/v24.11.1/bin:$PATH"
npx wrangler pages deploy . --project-name=grand-line --branch=main --commit-dirty=true
```

## Struttura

```
index.html  - tutto in un file (HTML + CSS inline + JS inline)
```
