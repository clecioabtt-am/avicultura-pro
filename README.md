# Avicultura Pro

Sistema web/PWA responsivo para gestão avícola, preparado para GitHub + Cloudflare Workers + D1.

## Nomes sugeridos
- Repositório GitHub: `avicultura-pro`
- Worker/Projeto Cloudflare: `avicultura-pro`
- Banco D1: `avicultura-pro-db`

## 1. Instalar dependências
```bash
npm install
```

## 2. Criar o banco D1
```bash
npx wrangler d1 create avicultura-pro-db
```
Copie o `database_id` retornado e substitua `COLE_AQUI_O_DATABASE_ID` em `wrangler.jsonc`.

## 3. Criar as tabelas no D1 remoto
```bash
npx wrangler d1 execute avicultura-pro-db --remote --file=./schema.sql
```

## 4. Rodar localmente
Para testar o front-end:
```bash
npm run dev
```
Para testar Worker + D1 local, primeiro:
```bash
npx wrangler d1 execute avicultura-pro-db --local --file=./schema.sql
npm run build
npx wrangler dev
```

## 5. Deploy
```bash
npm run deploy
```
Ou conecte o repositório ao Cloudflare e use:
- Build command: `npm run build`
- Deploy command: `npx wrangler deploy`

## Estrutura inicial já funcional
- Dashboard com indicadores reais lidos do D1
- Cadastro/listagem de lotes
- Cálculo automático da taxa de postura no dashboard
- Alertas de ração e vacina
- Tema claro/escuro
- Layout responsivo para celular, tablet e desktop
- Manifest + service worker para instalação como PWA e cache básico
- Tabelas D1 preparadas para produção de ovos, frangos, mortalidade, ração, saúde, clientes, vendas, despesas e usuários

## Observação importante sobre offline
Esta versão possui PWA e cache da interface. Para lançamento em produção com operação offline completa (incluindo cadastros sem internet e sincronização posterior), implemente uma fila em IndexedDB com resolução de conflitos. Não trate o cache básico do service worker como sincronização transacional.

## Segurança
A tabela `users` está preparada, mas autenticação não foi habilitada nesta versão inicial. Antes de colocar dados reais de clientes na internet, adicione autenticação e autorização por perfil no Worker.
