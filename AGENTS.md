# Versátil — guia para agentes

Secretária virtual com IA, adaptável a qualquer nicho. UI e textos em **português do Brasil**.

## Arquitetura

- `src/routes/index.tsx` — landing + assistente de criação (escolha de nicho → ajuste de campos → `createSecretary`).
- `src/routes/s.$publicId.tsx` — página pública de atendimento (chat do cliente).
- `src/routes/painel.$adminKey.tsx` — painel do dono: pedidos, configuração, chat de teste, link de compartilhamento.
- `src/routes/api.chat.ts` → `src/lib/chat.server.ts` — endpoint SSE do chat. Recebe `secretaryId` (publicId) em `forwardedProps`, carrega a secretária no banco, monta o prompt e expõe a ferramenta `registrarSolicitacao`.
- `src/lib/prompt.ts` — monta o system prompt a partir da configuração.
- `src/lib/secretary-tools.server.ts` — ferramenta que grava pedidos na tabela `requests`.
- `src/lib/secretary.functions.ts` — server functions (criar, ler público, painel, atualizar, mudar status).
- `src/lib/niches.ts` — presets de nicho (serviços, FAQ, regras, tom, cor, sugestões de conversa). Para adicionar um nicho, inclua um item aqui e, se precisar, um ícone em `src/components/NicheIcon.tsx`.
- `src/lib/my-secretaries.ts` — lista local (localStorage) dos painéis criados no navegador.
- `db/schema.ts` / `db/index.ts` — Drizzle + Netlify Database. Migrações em `netlify/database/migrations/` (gere com `drizzle-kit generate --name ...`; nunca edite migrações aplicadas).

## Decisões não óbvias

- **Sem login por enquanto**: cada secretária tem `publicId` (link do cliente) e `adminKey` (link secreto do painel). Toda server function de escrita valida `adminKey`. Nunca retorne `adminKey` em respostas públicas.
- Acesso ao banco nos handlers usa `import()` dinâmico para não vazar código de servidor no bundle do cliente.
- A secretária não consulta agenda real; ela coleta o horário desejado e a equipe confirma no painel.
- Drizzle precisa da tag `@beta` (`drizzle-orm@beta`, `drizzle-kit@beta`).
- Manter TypeScript 6 (TS 7 quebra o adaptador Netlify).
- Estilo: tokens em `src/styles.css` (`paper`, `ink`, `line`, `brand`, fontes Instrument Serif + Manrope); utilitários `.field`, `.label`, `.btn-ink`, `.btn-ghost`.
