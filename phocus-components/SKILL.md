---
name: phocus-components
description: |
  Biblioteca de componentes padrão Phocus — entrega o código pronto de Sidebar com nav dinâmico por perfil, página de Clientes (CRUD inline) e página de Usuários (formulário lateral + tabela), incluindo API routes e schema Prisma. Use quando o usuário disser "/components", "adicionar clientes", "adicionar usuários", "quero a tela padrão de usuários", "implementar sidebar Phocus", "padrão de menu lateral", ou sempre que um app novo precisar desses módulos. Ative também durante o /spec ou /execute quando o app tiver gestão de clientes ou usuários — copie os templates em vez de reescrever do zero. Se o projeto for Phocus e tiver qualquer área autenticada, aplique a Sidebar padrão automaticamente.
---

# /components — Componentes Padrão Phocus

Você está entregando os **componentes reutilizáveis** da plataforma Phocus.

Estes componentes estão validados em produção (checklist-hub, crono-maker). A consistência entre apps é o objetivo — quem usa um app Phocus deve reconhecer instantaneamente o outro.

---

## Passo 1 — Pergunte o que o usuário quer incluir

Antes de gerar qualquer código, apresente as opções disponíveis e pergunte quais incluir. **Não assuma — pergunte sempre.**

Apresente exatamente assim:

```
Quais componentes padrão Phocus você quer incluir neste app?

1. ✅ Sidebar + Layout  — menu lateral escuro, logo Phocus, nav por perfil (recomendado para todo app com login)
2. 👥 Gestão de Usuários — formulário lateral + tabela com CRUD completo (API routes + schema Prisma incluídos)
3. 🏢 Gestão de Clientes — CRUD de carteira de clientes da agência (API routes + schema Prisma incluídos)

Pode responder com os números (ex: "1 e 2") ou "todos".
```

Só avance para o Passo 2 depois de receber a resposta.

> **Regra de contexto:** Se você já sabe pelo PRD ou spec que o app é interno da agência e tem login, pode sugerir os três como padrão — mas ainda confirme antes de implementar.

---

## Passo 2 — Resolva o logo Phocus automaticamente

Antes de implementar qualquer componente, verifique se o logo já existe no projeto atual:

```bash
ls public/logos/
```

**Se o arquivo `LOGO PHOCUS BRANCA.png` já existir:** siga em frente sem fazer nada.

**Se não existir:** procure nos outros projetos Phocus no mesmo diretório pai e copie a pasta inteira de logos:

```bash
# Estratégia 1: tentar checklist-hub (fonte canônica dos logos)
cp -r ../checklist-hub/public/logos public/logos

# Se não encontrar, tentar crono-maker
cp -r ../crono-maker/public/logos public/logos

# Se não encontrar em nenhum, buscar em qualquer projeto irmão
find .. -name "LOGO PHOCUS BRANCA.png" -maxdepth 4 2>/dev/null | head -1
```

Se encontrar via `find`, copie a pasta `logos/` pai desse arquivo para `public/logos/`.

**Se não encontrar em nenhum lugar:** avise o usuário:
```
⚠️ Logo Phocus não encontrado automaticamente.
Por favor, copie a pasta /logos para public/logos/ manualmente.
O logo fica em: checklist-hub/public/logos/LOGO PHOCUS BRANCA.png
```

> Os logos que devem estar na pasta: `LOGO PHOCUS BRANCA.png`, `LOGO MAXI BRANCA.png`, `LOGO FAZ BRANCA.png`, `LOGO MAXI.png`, `LOGO FAZ-04.png`, `LOGO PH.png`

---

## Passo 3 — Verifique as CSS variables

Confirme que `app/globals.css` contém as variáveis de cor Phocus. Se não existirem, adicione:

```css
:root {
  --color-primary: #B0A2F9;      /* roxo Phocus — botões, links ativos */
  --color-bg: #F9F9F9;           /* fundo geral */
  --color-surface: #FFFFFF;      /* cards, tabelas */
  --color-border: #E5E5E5;       /* bordas */
  --color-text: #191818;         /* texto principal */
  --color-success: #45B577;      /* status Finalizado */
  --color-warning: #EE7D00;      /* alertas, Remover */
}
```

**Nunca hardcode cores nos componentes** — use sempre as variáveis acima.

---

## Passo 4 — Implemente os componentes selecionados

Implemente apenas os módulos que o usuário confirmou no Passo 1.

---

### COMPONENTE A — Sidebar + Dashboard Layout

**Arquivo:** `app/(dashboard)/layout.tsx`

Este layout envolve todas as rotas autenticadas. Exibe sidebar escura com logo branca Phocus, navegação dinâmica por perfil (ADMIN vê itens extras) e UserButton do Clerk no rodapé.

**Como adaptar:**
- Substitua os arrays `NAV` e `NAV_ADMIN` pelas rotas do app
- O endpoint `/api/me` deve retornar `{ perfil: string }` — implemente se não existir
- A logo sempre usa `/logos/LOGO PHOCUS BRANCA.png` (fundo escuro)

```tsx
'use client'

import Link from 'next/link'
import { usePathname } from 'next/navigation'
import { useUser, UserButton } from '@clerk/nextjs'
import { useEffect, useState } from 'react'

// ✏️ ADAPTAR: rotas principais do app
const NAV = [
  { href: '/campanhas', label: 'Campanhas' },
  { href: '/clientes', label: 'Clientes' },
  { href: '/admin/tipos-peca', label: 'Tipos de Peça' },
]

// ✏️ ADAPTAR: rotas exclusivas de ADMIN
const NAV_ADMIN = [
  { href: '/admin/usuarios', label: 'Usuários' },
  { href: '/admin/status-config', label: 'Status por Área' },
]

function LogoPhocus() {
  return (
    <img
      src="/logos/LOGO PHOCUS BRANCA.png"
      alt="Phocus"
      width={120}
      style={{ objectFit: 'contain' }}
    />
  )
}

export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  const pathname = usePathname()
  const { user } = useUser()
  const [perfil, setPerfil] = useState<string>('')

  useEffect(() => {
    fetch('/api/me')
      .then((r) => (r.ok ? r.json() : null))
      .then((d) => { if (d?.perfil) setPerfil(d.perfil) })
  }, [user?.id])

  const isAdmin = perfil === 'ADMIN'
  const navItems = isAdmin ? [...NAV, ...NAV_ADMIN] : NAV

  return (
    <div className="flex min-h-screen" style={{ background: 'var(--color-bg)' }}>
      {/* Sidebar */}
      <aside className="w-56 flex flex-col shrink-0" style={{ background: '#191818' }}>

        {/* Logo */}
        <div className="px-5 py-6 border-b" style={{ borderColor: 'rgba(255,255,255,0.08)' }}>
          <LogoPhocus />
        </div>

        {/* Navegação */}
        <nav className="flex-1 py-6 flex flex-col gap-1 px-3">
          {navItems.map((item) => {
            const active = pathname.startsWith(item.href)
            return (
              <Link
                key={item.href}
                href={item.href}
                className="px-3 py-2 rounded-lg text-sm font-semibold transition-colors"
                style={{
                  background: active ? 'var(--color-primary)' : 'transparent',
                  color: active ? '#191818' : 'rgba(249,249,249,0.7)',
                }}
              >
                {item.label}
              </Link>
            )
          })}
        </nav>

        {/* Usuário logado */}
        <div
          className="px-4 py-4 border-t flex items-center gap-3"
          style={{ borderColor: 'rgba(255,255,255,0.08)' }}
        >
          <UserButton />
          <span className="text-xs truncate" style={{ color: 'rgba(249,249,249,0.6)' }}>
            {user?.firstName ?? 'Usuário'}
          </span>
        </div>
      </aside>

      {/* Conteúdo principal */}
      <main className="flex-1 flex flex-col min-w-0 overflow-x-auto">
        {children}
      </main>
    </div>
  )
}
```

**API obrigatória para o layout funcionar:**

```typescript
// app/api/me/route.ts
import { auth } from '@clerk/nextjs/server'
import { getCurrentUser } from '@/lib/auth'

export async function GET() {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!user) return Response.json({ error: 'Usuário não encontrado' }, { status: 404 })
  return Response.json({ perfil: user.perfil, nome: user.nome, email: user.email })
}
```

---

### COMPONENTE B — Página de Clientes

**Arquivo:** `app/(dashboard)/clientes/page.tsx`

CRUD completo: adicionar cliente por nome, listar em tabela, editar inline na célula.

**Como adaptar:**
- Campos do model `Cliente`: `id`, `nome`, `criado_em` — adicione outros se necessário
- A API espera `/api/clientes` (GET, POST) e `/api/clientes/[id]` (PUT)
- Permissão de escrita: apenas ADMIN — verificar na API route, nunca no frontend

```tsx
'use client'

import { useEffect, useState } from 'react'

interface Cliente {
  id: string
  nome: string
  criado_em: string
}

export default function ClientesPage() {
  const [clientes, setClientes] = useState<Cliente[]>([])
  const [loading, setLoading] = useState(true)
  const [novoNome, setNovoNome] = useState('')
  const [salvando, setSalvando] = useState(false)
  const [erro, setErro] = useState('')
  const [editando, setEditando] = useState<{ id: string; nome: string } | null>(null)

  async function carregar() {
    const res = await fetch('/api/clientes')
    if (res.ok) setClientes(await res.json())
    setLoading(false)
  }

  useEffect(() => { carregar() }, [])

  async function handleCriar(e: React.FormEvent) {
    e.preventDefault()
    setErro('')
    if (!novoNome.trim()) { setErro('Nome é obrigatório.'); return }
    setSalvando(true)
    const res = await fetch('/api/clientes', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ nome: novoNome.trim() }),
    })
    setSalvando(false)
    if (res.ok) { setNovoNome(''); carregar() }
    else { const d = await res.json(); setErro(d.error ?? 'Erro ao criar.') }
  }

  async function handleEditar(e: React.FormEvent) {
    e.preventDefault()
    if (!editando) return
    const res = await fetch(`/api/clientes/${editando.id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ nome: editando.nome }),
    })
    if (res.ok) { setEditando(null); carregar() }
  }

  return (
    <div className="p-8 max-w-2xl">
      <div className="mb-8">
        <h1 className="text-2xl font-black" style={{ color: 'var(--color-text)' }}>Clientes</h1>
        <p className="text-sm mt-1" style={{ color: 'rgba(25,24,24,0.5)' }}>
          Cadastre e gerencie os clientes da agência.
        </p>
      </div>

      <form onSubmit={handleCriar} className="flex gap-2 mb-8">
        <input
          type="text"
          value={novoNome}
          onChange={(e) => setNovoNome(e.target.value)}
          placeholder="Nome do cliente"
          className="flex-1 px-3 py-2 rounded-lg border text-sm outline-none"
          style={{ borderColor: 'var(--color-border)', background: 'var(--color-surface)' }}
        />
        <button
          type="submit"
          disabled={salvando}
          className="px-4 py-2 rounded-lg text-sm font-semibold disabled:opacity-50"
          style={{ background: 'var(--color-primary)', color: '#191818' }}
        >
          {salvando ? 'Salvando...' : 'Adicionar'}
        </button>
      </form>
      {erro && <p className="text-xs mb-4" style={{ color: 'var(--color-warning)' }}>{erro}</p>}

      {loading ? (
        <p className="text-sm" style={{ color: 'rgba(25,24,24,0.4)' }}>Carregando...</p>
      ) : (
        <div className="rounded-xl border overflow-hidden" style={{ background: 'var(--color-surface)', borderColor: 'var(--color-border)' }}>
          {clientes.length === 0 ? (
            <p className="p-6 text-sm text-center" style={{ color: 'rgba(25,24,24,0.3)' }}>
              Nenhum cliente cadastrado ainda.
            </p>
          ) : (
            <table className="w-full text-sm">
              <thead>
                <tr className="border-b text-left" style={{ borderColor: 'var(--color-border)', background: 'var(--color-bg)' }}>
                  <th className="px-4 py-3 font-semibold" style={{ color: 'rgba(25,24,24,0.6)' }}>Nome</th>
                  <th className="px-4 py-3 font-semibold" style={{ color: 'rgba(25,24,24,0.6)' }}>Criado em</th>
                  <th className="px-4 py-3 w-24"></th>
                </tr>
              </thead>
              <tbody className="divide-y" style={{ borderColor: 'var(--color-border)' }}>
                {clientes.map((c) => (
                  <tr key={c.id} className="hover:bg-gray-50">
                    <td className="px-4 py-3 font-medium" style={{ color: 'var(--color-text)' }}>
                      {editando?.id === c.id ? (
                        <form onSubmit={handleEditar} className="flex gap-2">
                          <input
                            autoFocus
                            value={editando.nome}
                            onChange={(e) => setEditando({ ...editando, nome: e.target.value })}
                            className="flex-1 px-2 py-1 rounded border text-sm outline-none"
                            style={{ borderColor: 'var(--color-primary)' }}
                          />
                          <button type="submit" className="text-xs font-semibold px-2 py-1 rounded" style={{ background: 'var(--color-primary)', color: '#191818' }}>OK</button>
                          <button type="button" onClick={() => setEditando(null)} className="text-xs px-2 py-1 rounded border" style={{ borderColor: 'var(--color-border)', color: 'rgba(25,24,24,0.5)' }}>×</button>
                        </form>
                      ) : c.nome}
                    </td>
                    <td className="px-4 py-3 text-sm" style={{ color: 'rgba(25,24,24,0.5)' }}>
                      {new Date(c.criado_em).toLocaleDateString('pt-BR')}
                    </td>
                    <td className="px-4 py-3 text-right">
                      <button onClick={() => setEditando({ id: c.id, nome: c.nome })} className="text-xs font-semibold" style={{ color: 'var(--color-primary)' }}>
                        Editar
                      </button>
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
          )}
        </div>
      )}
    </div>
  )
}
```

**API routes para Clientes:**

```typescript
// app/api/clientes/route.ts
import { auth } from '@clerk/nextjs/server'
import { getCurrentUser } from '@/lib/auth'
import { isAdmin } from '@/lib/roles'
import { clienteRepository } from '@/repositories/clienteRepository'

export async function GET() {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const clientes = await clienteRepository.findAll()
  return Response.json(clientes)
}

export async function POST(request: Request) {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!isAdmin(user)) return Response.json({ error: 'Sem permissão' }, { status: 403 })
  const { nome } = await request.json()
  if (!nome?.trim()) return Response.json({ error: 'Nome é obrigatório.' }, { status: 400 })
  const cliente = await clienteRepository.create({ nome: nome.trim() })
  return Response.json(cliente, { status: 201 })
}

// app/api/clientes/[id]/route.ts
export async function PUT(request: Request, { params }: { params: { id: string } }) {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!isAdmin(user)) return Response.json({ error: 'Sem permissão' }, { status: 403 })
  const { nome } = await request.json()
  if (!nome?.trim()) return Response.json({ error: 'Nome é obrigatório.' }, { status: 400 })
  const cliente = await clienteRepository.update(params.id, { nome: nome.trim() })
  return Response.json(cliente)
}
```

**Schema Prisma para Clientes:**

```prisma
model Cliente {
  id        String   @id @default(cuid())
  nome      String
  criado_em DateTime @default(now())
  // ✏️ ADAPTAR: adicionar relações se necessário
}
```

---

### COMPONENTE C — Página de Usuários (Admin)

**Arquivo:** `app/(dashboard)/admin/usuarios/page.tsx`

Formulário lateral (criar/editar) + tabela completa. Badge de perfil colorido. Campo de área para perfil HEAD.

**Como adaptar:**
- `PERFIS`: ajuste os perfis do app
- `AREAS` e `AREA_LABELS`: remova se o app não tiver área por perfil
- A API espera `/api/admin/usuarios` (GET, POST) e `/api/admin/usuarios/[id]` (PUT, DELETE)

```tsx
'use client'

import { useEffect, useState } from 'react'

// ✏️ ADAPTAR: perfis do app
const PERFIS = ['ADMIN', 'GC', 'HEAD', 'CLIENTE']

// ✏️ ADAPTAR: remover se o app não tiver área por perfil
const AREAS = ['CRIACAO_ON', 'CRIACAO_OFF', 'PRODUCAO', 'MIDIA_OFF', 'MIDIA_ON', 'DIGITAL']
const AREA_LABELS: Record<string, string> = {
  CRIACAO_ON: 'Criação ON', CRIACAO_OFF: 'Criação OFF', PRODUCAO: 'Produção',
  MIDIA_OFF: 'Mídia OFF', MIDIA_ON: 'Mídia ON', DIGITAL: 'Digital',
}

interface Usuario { id: string; nome: string; email: string; perfil: string; area_head: string | null; clerk_user_id: string }
interface FormData { nome: string; email: string; clerk_user_id: string; perfil: string; area_head: string }
const FORM_INICIAL: FormData = { nome: '', email: '', clerk_user_id: '', perfil: 'GC', area_head: '' }

export default function AdminUsuariosPage() {
  const [usuarios, setUsuarios] = useState<Usuario[]>([])
  const [loading, setLoading] = useState(true)
  const [form, setForm] = useState<FormData>(FORM_INICIAL)
  const [editando, setEditando] = useState<string | null>(null)
  const [salvando, setSalvando] = useState(false)
  const [erro, setErro] = useState('')

  async function carregar() {
    const res = await fetch('/api/admin/usuarios')
    if (res.ok) setUsuarios(await res.json())
    setLoading(false)
  }

  useEffect(() => { carregar() }, [])

  async function handleSubmit(e: React.FormEvent) {
    e.preventDefault()
    if (!form.nome.trim() || !form.email.trim() || !form.clerk_user_id.trim()) {
      setErro('Nome, email e Clerk User ID são obrigatórios.'); return
    }
    setSalvando(true); setErro('')
    const payload = {
      nome: form.nome.trim(), email: form.email.trim(), clerk_user_id: form.clerk_user_id.trim(),
      perfil: form.perfil,
      area_head: form.perfil === 'HEAD' && form.area_head ? form.area_head : undefined,
    }
    const url = editando ? `/api/admin/usuarios/${editando}` : '/api/admin/usuarios'
    const res = await fetch(url, { method: editando ? 'PUT' : 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) })
    if (res.ok) { setForm(FORM_INICIAL); setEditando(null); carregar() }
    else { const d = await res.json(); setErro(d.error ?? 'Erro ao salvar') }
    setSalvando(false)
  }

  function handleEditar(u: Usuario) {
    setEditando(u.id)
    setForm({ nome: u.nome, email: u.email, clerk_user_id: u.clerk_user_id, perfil: u.perfil, area_head: u.area_head ?? '' })
    setErro('')
  }

  async function handleDeletar(id: string) {
    if (!confirm('Remover este usuário?')) return
    await fetch(`/api/admin/usuarios/${id}`, { method: 'DELETE' })
    carregar()
  }

  return (
    <div className="p-8">
      <h1 className="text-2xl font-black mb-8" style={{ color: 'var(--color-text)' }}>Usuários</h1>
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
        {/* Formulário lateral */}
        <div className="rounded-xl border p-6" style={{ background: 'var(--color-surface)', borderColor: 'var(--color-border)' }}>
          <h2 className="text-base font-black mb-5" style={{ color: 'var(--color-text)' }}>
            {editando ? 'Editar usuário' : 'Novo usuário'}
          </h2>
          <form onSubmit={handleSubmit} className="flex flex-col gap-4">
            {[
              { key: 'nome', label: 'Nome *', placeholder: 'Nome completo' },
              { key: 'email', label: 'E-mail *', placeholder: 'email@exemplo.com' },
              { key: 'clerk_user_id', label: 'Clerk User ID *', placeholder: 'user_...' },
            ].map(({ key, label, placeholder }) => (
              <div key={key} className="flex flex-col gap-1">
                <label className="text-xs font-semibold" style={{ color: 'rgba(25,24,24,0.6)' }}>{label}</label>
                <input type="text" value={form[key as keyof FormData]}
                  onChange={(e) => setForm({ ...form, [key]: e.target.value })}
                  placeholder={placeholder}
                  className="px-3 py-2 rounded-lg border text-sm outline-none"
                  style={{ borderColor: 'var(--color-border)', background: 'var(--color-bg)' }}
                />
              </div>
            ))}
            <div className="flex flex-col gap-1">
              <label className="text-xs font-semibold" style={{ color: 'rgba(25,24,24,0.6)' }}>Perfil *</label>
              <select value={form.perfil} onChange={(e) => setForm({ ...form, perfil: e.target.value, area_head: '' })}
                className="px-3 py-2 rounded-lg border text-sm outline-none"
                style={{ borderColor: 'var(--color-border)', background: 'var(--color-bg)' }}>
                {PERFIS.map((p) => <option key={p} value={p}>{p}</option>)}
              </select>
            </div>
            {form.perfil === 'HEAD' && (
              <div className="flex flex-col gap-1">
                <label className="text-xs font-semibold" style={{ color: 'rgba(25,24,24,0.6)' }}>Área do Head</label>
                <select value={form.area_head} onChange={(e) => setForm({ ...form, area_head: e.target.value })}
                  className="px-3 py-2 rounded-lg border text-sm outline-none"
                  style={{ borderColor: 'var(--color-border)', background: 'var(--color-bg)' }}>
                  <option value="">Selecionar área</option>
                  {AREAS.map((a) => <option key={a} value={a}>{AREA_LABELS[a]}</option>)}
                </select>
              </div>
            )}
            {erro && <p className="text-xs" style={{ color: 'var(--color-warning)' }}>{erro}</p>}
            <div className="flex gap-2 pt-2">
              {editando && (
                <button type="button" onClick={() => { setEditando(null); setForm(FORM_INICIAL); setErro('') }}
                  className="flex-1 py-2 rounded-lg text-sm border font-semibold"
                  style={{ borderColor: 'var(--color-border)', color: 'rgba(25,24,24,0.5)' }}>
                  Cancelar
                </button>
              )}
              <button type="submit" disabled={salvando}
                className="flex-1 py-2 rounded-lg text-sm font-semibold disabled:opacity-50"
                style={{ background: 'var(--color-primary)', color: '#191818' }}>
                {salvando ? 'Salvando...' : editando ? 'Atualizar' : 'Criar usuário'}
              </button>
            </div>
          </form>
        </div>

        {/* Tabela */}
        <div className="lg:col-span-2">
          <div className="rounded-xl border overflow-hidden" style={{ background: 'var(--color-surface)', borderColor: 'var(--color-border)' }}>
            {loading ? (
              <div className="p-8 text-sm text-center" style={{ color: 'rgba(25,24,24,0.4)' }}>Carregando...</div>
            ) : usuarios.length === 0 ? (
              <div className="p-8 text-sm text-center" style={{ color: 'rgba(25,24,24,0.3)' }}>Nenhum usuário cadastrado.</div>
            ) : (
              <table className="w-full text-sm">
                <thead>
                  <tr className="border-b text-left" style={{ borderColor: 'var(--color-border)' }}>
                    {['Nome', 'E-mail', 'Perfil', 'Área', ''].map((h) => (
                      <th key={h} className="px-4 py-3 font-semibold" style={{ color: 'rgba(25,24,24,0.5)' }}>{h}</th>
                    ))}
                  </tr>
                </thead>
                <tbody>
                  {usuarios.map((u) => (
                    <tr key={u.id} className="border-b hover:bg-gray-50" style={{ borderColor: 'var(--color-border)' }}>
                      <td className="px-4 py-3 font-semibold" style={{ color: 'var(--color-text)' }}>{u.nome}</td>
                      <td className="px-4 py-3" style={{ color: 'rgba(25,24,24,0.6)' }}>{u.email}</td>
                      <td className="px-4 py-3">
                        <span className="text-xs font-semibold px-2 py-1 rounded-full" style={{ background: '#B0A2F922', color: '#6b5fd4' }}>
                          {u.perfil}
                        </span>
                      </td>
                      <td className="px-4 py-3 text-xs" style={{ color: 'rgba(25,24,24,0.5)' }}>
                        {u.area_head ? (AREA_LABELS[u.area_head] ?? u.area_head) : '—'}
                      </td>
                      <td className="px-4 py-3">
                        <div className="flex gap-2 justify-end">
                          <button onClick={() => handleEditar(u)} className="text-xs px-2 py-1 rounded border font-semibold"
                            style={{ borderColor: 'var(--color-border)', color: 'rgba(25,24,24,0.6)' }}>Editar</button>
                          <button onClick={() => handleDeletar(u.id)} className="text-xs px-2 py-1 rounded border font-semibold"
                            style={{ borderColor: 'var(--color-warning)', color: 'var(--color-warning)' }}>Remover</button>
                        </div>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            )}
          </div>
        </div>
      </div>
    </div>
  )
}
```

**API routes para Usuários:**

```typescript
// app/api/admin/usuarios/route.ts
import { auth } from '@clerk/nextjs/server'
import { getCurrentUser } from '@/lib/auth'
import { isAdmin } from '@/lib/roles'
import { usuarioRepository } from '@/repositories/usuarioRepository'

export async function GET() {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!isAdmin(user)) return Response.json({ error: 'Sem permissão' }, { status: 403 })
  return Response.json(await usuarioRepository.findAll())
}

export async function POST(request: Request) {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!isAdmin(user)) return Response.json({ error: 'Sem permissão' }, { status: 403 })
  const { nome, email, clerk_user_id, perfil, area_head } = await request.json()
  if (!nome?.trim() || !email?.trim() || !clerk_user_id?.trim())
    return Response.json({ error: 'Nome, email e Clerk User ID são obrigatórios.' }, { status: 400 })
  return Response.json(await usuarioRepository.create({ nome, email, clerk_user_id, perfil, area_head }), { status: 201 })
}

// app/api/admin/usuarios/[id]/route.ts
export async function PUT(request: Request, { params }: { params: { id: string } }) {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!isAdmin(user)) return Response.json({ error: 'Sem permissão' }, { status: 403 })
  return Response.json(await usuarioRepository.update(params.id, await request.json()))
}

export async function DELETE(_: Request, { params }: { params: { id: string } }) {
  const { userId } = auth()
  if (!userId) return Response.json({ error: 'Não autenticado' }, { status: 401 })
  const user = await getCurrentUser(userId)
  if (!isAdmin(user)) return Response.json({ error: 'Sem permissão' }, { status: 403 })
  await usuarioRepository.delete(params.id)
  return new Response(null, { status: 204 })
}
```

**Schema Prisma para Usuários:**

```prisma
model Usuario {
  id            String   @id @default(cuid())
  nome          String
  email         String   @unique
  clerk_user_id String   @unique
  perfil        String   // ADMIN | GC | HEAD | CLIENTE
  area_head     String?  // Apenas para perfil HEAD
  criado_em     DateTime @default(now())
  atualizado_em DateTime @updatedAt
  // ✏️ ADAPTAR: adicionar relações com outras entidades do app
}
```

---

## Passo 5 — Confirme e oriente

Após implementar os componentes selecionados, informe o status:

```
✅ Componentes Phocus instalados:

  Logo: [✅ encontrado e copiado automaticamente | ⚠️ não encontrado — cópia manual necessária]
  CSS variables: [✅ já existiam | ✅ adicionadas ao globals.css]

  [✅ Sidebar + Layout]         → app/(dashboard)/layout.tsx
  [✅ Gestão de Clientes]       → app/(dashboard)/clientes/page.tsx + /api/clientes/
  [✅ Gestão de Usuários]       → app/(dashboard)/admin/usuarios/page.tsx + /api/admin/usuarios/

Adapte os arrays NAV e NAV_ADMIN no layout com as rotas reais do app.
Após migrar o schema: npx prisma migrate dev --name add-[componentes]
```

---

## Red Flags — Parar e alertar

- ❌ Rota `/api/admin/*` sem verificar `isAdmin()` → falha de segurança grave
- ❌ `clerk_user_id` hardcoded → sempre vem do formulário ou Clerk Dashboard
- ❌ Sidebar sem verificar perfil → usuário comum veria rotas de admin
- ❌ Deletar sem `confirm()` → UX perigoso
- ❌ Importar `@prisma/client` diretamente → usar sempre repository
- ❌ Logo não encontrado e implementação prosseguindo sem avisar → informar o usuário
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 