# Granja Amazonas — Gestão Avícola

Sistema web/PWA responsivo para gestão de avicultura, preparado para Cloudflare Workers + D1.

## Nomes recomendados
- GitHub: `granja-amazonas`
- Worker/Cloudflare: `granja-amazonas`
- D1: `granja-amazonas-db`

> Se preferir manter o nome técnico antigo, `avicultura-pro` também funciona. Ajuste `name` e `database_name` no `wrangler.jsonc`.

## Módulos funcionais
Dashboard, lotes, poedeiras, frangos de corte, ração/estoque e consumo, saúde/manejo, mortalidade, reprodução/incubação, clientes, vendas, financeiro, relatórios, usuários/permissões e configurações com troca de logo.

## Segurança
Na primeira abertura o sistema solicita a criação da conta da proprietária. Senhas usam PBKDF2-SHA256 e sessões usam cookie HttpOnly/Secure. Proprietária e gerente acessam usuários/configurações; funcionário fica com módulos operacionais.

## Instalação
```bash
npm install
npm run build
```

## Criar o D1
```bash
npx wrangler d1 create granja-amazonas-db
```
Copie o `database_id` retornado e coloque no `wrangler.jsonc`.

## Criar tabelas
```bash
npx wrangler d1 execute granja-amazonas-db --remote --file=./schema.sql
```

## Publicar
```bash
npm run build
npx wrangler deploy
```

## Desenvolvimento local
```bash
npm run db:local
npm run dev
```

## Logo
A logo fornecida está em `public/logo.jpg`. Depois do primeiro acesso, a proprietária pode alterá-la em **Configurações > Identidade da granja**. A imagem personalizada é armazenada nas configurações do D1.

## Relatórios
A tela de Relatórios permite períodos diário, semanal e mensal, exportação CSV/planilha e impressão/Salvar como PDF pelo navegador.

## Correção 1.0.1 — criação do primeiro acesso

Esta versão corrige o erro HTTP 500 em `POST /api/auth/setup` observado na configuração inicial.

Alterações principais:
- criação da proprietária passa a criar a sessão no mesmo request;
- a interface não executa um segundo login após o setup;
- inicialização de autenticação verifica e corrige a coluna `password_salt` quando necessário;
- tabela `sessions` é garantida antes da autenticação;
- hashing de senha recebeu compatibilidade com a versão anterior e fallback para ambientes com limitação do PBKDF2;
- Worker e scripts foram alinhados ao projeto `avicultura-pro` e banco `avicultura-pro-db`.

### Atenção ao binding D1

Confirme no `wrangler.jsonc` se o `database_id` corresponde ao seu banco **avicultura-pro-db** no Cloudflare. O nome do binding deve permanecer exatamente `DB`.
