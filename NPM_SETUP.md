# Publicando @sentinel-ai/sdk no npm

## Configuração única

1. Crie uma conta npm em https://www.npmjs.com/signup (caso não tenha uma)

2. Crie um token de acesso npm:
   - Acesse https://www.npmjs.com/settings/~/tokens
   - Clique em "Generate New Token" > "Classic Token"
   - Selecione o tipo "Automation"
   - Copie o token

3. Adicione o token ao GitHub:
   - Acesse https://github.com/MaxwellCalkin/sentinel-ai/settings/secrets/actions
   - Clique em "New repository secret"
   - Nome: `NPM_TOKEN`
   - Valor: cole o token do npm

4. Crie a org do npm (opcional, para pacotes com escopo):
   - Acesse https://www.npmjs.com/org/create
   - Crie a org: `sentinel-ai`
   - Ou publique sem escopo alterando o `name` em `sdk-js/package.json` para `sentinel-guardrails-js`

## Publicação

### Automática (em release do GitHub)
O workflow de publicação no npm roda automaticamente quando você cria um release no GitHub. A versão do SDK JS em `sdk-js/package.json` deve corresponder à tag do release.

### Manual
```bash
cd sdk-js
npm login
npm publish --access public
```

## O que é publicado

O pacote inclui:
- `dist/index.js` — JavaScript compilado (CommonJS)
- `dist/index.d.ts` — declarações de tipos TypeScript
- `README.md` — documentação do pacote
- `LICENSE` — Apache 2.0

O código-fonte (`src/`) e os testes NÃO estão incluídos no pacote npm.
