# Alana — secretária virtual multi-tenant

Alana é uma plataforma de secretárias virtuais com IA. Cada cliente tem seu próprio escritório digital: configura a secretária do negócio, recebe leads e (em breve) conecta o WhatsApp próprio.

## O que tem nesta versão (MVP)

- **Auth** com e-mail + senha (admin da plataforma + clientes)
- **Dashboard do cliente**: cria/edita secretária, vê leads, baixa CSV, exclui leads
- **Dashboard admin**: lista clientes, ativa/desativa, escolhe provedor de IA global
- **Provedores de IA**: Gemini, Groq e OpenAI (selecionável no admin)
- **Chat público** por secretária (`/s/<publicId>`)
- Preparação de modelo para WhatsApp (Evolution/Baileys) — conexão real na próxima fase

## Rotas principais

| Rota | Descrição |
|------|-----------|
| `/` | Landing |
| `/register` | Cadastro de cliente |
| `/login` | Login |
| `/dashboard` | Escritório do cliente |
| `/admin` | Painel da plataforma |
| `/setup` | Criar o primeiro admin (uma vez) |
| `/s/$publicId` | Chat público da secretária |

## Tecnologias

- TanStack Start (React 19) + Tailwind CSS 4
- TanStack AI (Gemini / OpenAI / Groq)
- Netlify Database (Postgres) + Drizzle ORM
- Hospedagem na Netlify

## Setup

```bash
pnpm install
# Configure as variáveis de ambiente no Netlify:
# - GROQ_API_KEY (se usar Groq)
# - OPENAI_API_KEY / GEMINI_API_KEY / GOOGLE_API_KEY
# - BOOTSTRAP_SECRET (opcional, padrão: alana-setup-2026)

netlify dev
```

1. Acesse `/setup` e crie o primeiro admin.
2. Depois use `/admin` para gerenciar e `/register` para clientes.

## Migrações

As migrações ficam em `netlify/database/migrations/`. Após alterar `db/schema.ts`:

```bash
npx drizzle-kit generate --name descricao_da_mudanca
```

## Próximos passos

- Integração real WhatsApp via Evolution API (precisa de serviço persistente fora da Netlify)
- Notificações de novos leads
- Múltiplas secretárias por cliente
- Widget embutível
