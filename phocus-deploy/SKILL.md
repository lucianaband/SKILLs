---
name: phocus-deploy
description: >
  Prepara um app Phocus (Next.js + Prisma + Tailwind v4) para deploy no Dokku (deploy.phocus.mxmz.app) via GitHub Actions, evitando os 3 erros que sempre quebram o build: OOM no type-check, falta de prisma generate, e binários nativos faltando no lockfile quando regenerado no Windows. Use sempre que o usuário disser "/phocus-deploy", "configurar deploy", "preparar para deploy", "deploy não passou", "deploy quebrou", "build do Dokku falhou", "OOM no build", "killed no type-check", "Cannot find module lightningcss", "Cannot find module @tailwindcss/oxide", "engines.node unspecified", "novo app no Dokku", ou quando estiver subindo um app pela primeira vez no servidor Phocus. Esta skill replica exatamente a configuração validada em crono-maker, checklist-hub e resumo-investimentos.
---

# Phocus Deploy — Configuração para Dokku

> Skill aplicada a apps Next.js que rodam em `deploy.phocus.mxmz.app` via Dokku + herokuish + GitHub Actions.
> Não usar fora desse contexto (Vercel, Railway, Render têm outros padrões).

---

## Quando usar

Use esta skill quando o usuário pedir para:
- Configurar deploy de um app Phocus pela primeira vez
- Corrigir um build do Dokku que falhou
- Replicar o setup de deploy do `crono-maker` em outro app

Não use se o app já tem `Procfile + app.json + .npmrc + engines + workflow` e o build está passando — neste caso, apenas leia os logs e corrija o erro específico.

---

## Os 3 erros que SEMPRE acontecem (em ordem)

Cada um vira erro no Dokku porque o `herokuish` é estrito: sem `engines`, ele pega Node mais novo; sem `prisma generate`, falta o client; sem binários nativos no lockfile, falta o `.node` na hora de buildar Tailwind/lightningcss.

### Erro 1 — OOM no type-check

**Sintoma no log do Dokku:**
```
Running TypeScript ...
Killed
-----> Build failed
```

**Causa raiz:** `engines.node` não declarado → herokuish sobe Node 24 → `next build` roda type-check com memória padrão → estoura.

**Correção:**
1. Pinar `engines: { node: "22.x", npm: "10.x" }` no `package.json`
2. Adicionar `typescript: { ignoreBuildErrors: true }` no `next.config.ts` (type-check roda no `next dev`, não no build de produção)

### Erro 2 — Prisma client ausente

**Sintoma no log do Dokku:** erro de import `@prisma/client` ou tipo `PrismaClient` indefinido durante o `next build`.

**Causa raiz:** `npm install` não roda `prisma generate` automaticamente em produção (Dokku define `NODE_ENV=production`, que desliga lifecycle scripts).

**Correção:** mudar o script `build` no `package.json` para:
```json
"build": "prisma generate && next build"
```

### Erro 3 — Binário nativo Linux faltando no lockfile

**Sintoma no log do Dokku:**
```
Cannot find module '../lightningcss.linux-x64-gnu.node'
Require stack:
- /tmp/build/node_modules/lightningcss/node/index.js
```
ou:
```
Cannot find module '@tailwindcss/oxide-linux-x64-gnu'
```

**Causa raiz:** `package-lock.json` foi regenerado no Windows → npm só lista os binários `win32-x64-msvc` resolvidos → no Linux, npm segue o lockfile e nem tenta baixar o binário Linux.

**Correção:** regenerar o lockfile incluindo binários Linux:
```bash
rm -f package-lock.json
rm -rf node_modules
npx -y npm@10 install --os=linux --cpu=x64 --include=optional
npx -y npm@10 install   # reinstala localmente, preservando entradas Linux no lockfile
```

Para validar, conferir que aparecem 3 ocorrências de cada binário Linux:
```bash
grep -c "lightningcss-linux-x64-gnu" package-lock.json    # esperado: 3
grep -c "@tailwindcss/oxide-linux-x64-gnu" package-lock.json  # esperado: 3
```
(1 ocorrência = só a declaração na `optionalDependencies`; 3 = declaração + entrada do pacote + checksum, indicando que o binário foi resolvido)

---

## Checklist completo de arquivos

Use exatamente este conteúdo. Ajustar apenas o nome do app onde indicado.

### 1. `package.json` — campos críticos

```json
{
  "scripts": {
    "build": "prisma generate && next build",
    "start": "next start"
  },
  "engines": {
    "node": "22.x",
    "npm": "10.x"
  },
  "dependencies": {
    "tsx": "^4.21.0"
  }
}
```

**Atenção:**
- `tsx` deve estar em `dependencies` (não devDependencies). Se o release hook usar `npx tsx`, o `npm prune --production` apaga ele e o release quebra.
- Não adicionar `dotenv` se o `prisma.config.ts` não importar — é dep desnecessária.

### 2. `next.config.ts` — type-check off no build

```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  typescript: { ignoreBuildErrors: true },
};

export default nextConfig;
```

**NÃO** adicionar `eslint: { ignoreDuringBuilds: true }` em Next 16 — a chave foi removida e gera warning `Unrecognized key`. Se o app tem Sentry (como crono-maker), envolver com `withSentryConfig` mantendo a mesma estrutura.

### 3. `Procfile` — comandos do Dokku

```
web: npm run start
release: npx prisma migrate deploy
```

Se o app precisa de backfills ou snapshots de DB (como crono-maker), apontar `release` para `bash scripts/deploy/release.sh` e criar o script seguindo o template do crono-maker (snapshot SQLite + migrate + backfill + validação + rollback em caso de erro).

### 4. `.npmrc` — resiliência do npm install

```
fetch-retry-mintimeout=20000
fetch-retry-maxtimeout=120000
legacy-peer-deps=false
```

### 5. `app.json` — healthcheck Dokku

```json
{
  "name": "NOME_DO_APP",
  "description": "DESCRICAO_CURTA",
  "scripts": {
    "dokku": {
      "predeploy": "echo 'Predeploy ok.'",
      "postdeploy": "echo 'Deploy concluido em' $(date -u +%Y-%m-%dT%H:%M:%SZ)"
    }
  },
  "healthchecks": {
    "web": [
      {
        "type": "startup",
        "name": "web responde HTTP 200",
        "path": "/",
        "attempts": 5,
        "wait": 6,
        "timeout": 10,
        "initialDelay": 5,
        "httpHeaders": [{ "name": "User-Agent", "value": "dokku-healthcheck" }]
      }
    ]
  }
}
```

### 6. `.github/workflows/deploy-NOME_DO_APP.yml` — workflow

Ficar na **raiz do monorepo** (`phocus-apps/.github/workflows/`), não dentro da pasta do app.

```yaml
name: "Deploy - NOME_DO_APP"

on:
  push:
    branches:
      - main
    paths:
      - "NOME_DO_APP/**"
      - ".github/workflows/deploy-NOME_DO_APP.yml"

env:
  APP_NAME: NOME_DO_APP
  SERVER: deploy.phocus.mxmz.app

jobs:
  deploy-site:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Deploy site to homolog via Dokku
        uses: dokku/github-action@master
        with:
          git_push_flags: "--force"
          branch: main
          git_remote_url: "ssh://dokku@${{ env.SERVER }}/${{ env.APP_NAME }}"
          ssh_private_key: ${{ secrets.HOMOLOG_PRIVATE_KEY }}
```

### 7. `package-lock.json` — regenerar com binários Linux

Sempre rodar **na ordem exata** abaixo (mesmo se você estiver no Windows):

```bash
cd APP_FOLDER
rm -f package-lock.json
rm -rf node_modules
npx -y npm@10 install --os=linux --cpu=x64 --include=optional
npx -y npm@10 install
```

A primeira chamada resolve os binários Linux no lockfile; a segunda repopula o `node_modules` local com binários do SO atual sem remover as entradas Linux do lock.

Validar:
```bash
grep -c "lightningcss-linux-x64-gnu" package-lock.json
grep -c "@tailwindcss/oxide-linux-x64-gnu" package-lock.json
# ambos devem retornar 3
```

---

## Fluxo de execução desta skill

Quando ativada, executar **nesta ordem**:

1. **Inspecionar o app:**
   - Ler `package.json` — tem `engines`? `build` chama `prisma generate`? `tsx` está em dependencies?
   - Ler `next.config.ts` — tem `ignoreBuildErrors`? Tem key `eslint` inválida?
   - Listar raiz do app — existe `Procfile`, `.npmrc`, `app.json`?
   - Listar `.github/workflows/` na raiz do monorepo — existe `deploy-NOME_DO_APP.yml`?
   - Rodar `grep -c "lightningcss-linux-x64-gnu" package-lock.json` — retornou 3?

2. **Aplicar correções faltantes** seguindo o checklist acima. Não tocar no que já está correto.

3. **Regenerar lockfile** com a sequência de comandos do item 7 (sempre, mesmo se o app já tinha Procfile — porque o lockfile pode estar Windows-only).

4. **Validar build local:**
   ```bash
   npm run build
   ```
   Se falhar, parar e mostrar o erro ao usuário antes de commitar.

5. **Commit e push** com mensagem no formato:
   ```
   fix(NOME_DO_APP/deploy): config Dokku padrao Phocus (engines, prisma generate, lockfile cross-platform)
   ```

6. **Avisar o usuário** para acompanhar em https://github.com/maximize/phocus-apps/actions

---

## Erros raros (fora dos 3 principais)

### `EBADENGINE` no `npm install` local
Aparece ao rodar `npm install` em Node 24+ porque `engines` está pinado em 22.x. É **warning**, não erro — pode ignorar. Em produção o herokuish vai usar Node 22 conforme o `engines`.

### `--force` no push do GitHub Action
O workflow usa `git_push_flags: "--force"` propositalmente. O Dokku tem seu próprio histórico git e o force-push é o jeito padrão de sincronizar.

### `EOL warning: LF will be replaced by CRLF`
Apenas warning do git no Windows. Sem impacto no deploy.

### Middleware deprecation warning no Next 16
```
The "middleware" file convention is deprecated. Please use "proxy" instead.
```
**É apenas warning, não bloqueia build.** Migrar para `proxy.ts` é uma tarefa separada — não fazer durante esta skill.

---

## Referência: apps já configurados com este padrão

- `crono-maker/` — referência canônica, com release hook complexo (snapshot + backfill + rollback)
- `checklist-hub/` — padrão similar
- `resumo-investimentos/` — primeira aplicação documentada desta skill

Quando em dúvida sobre algum arquivo, comparar com `crono-maker/` — é a fonte de verdade.
