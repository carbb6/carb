# CarbonPath 2050 — IA gratuita (Cloudflare Pages + Workers AI)

Este pacote reaproveita o HTML original do CarbonPath 2050 e adiciona uma aba **IA gratuita** ao assistente. A IA recebe um resumo dos dados atuais do simulador e responde em português. Os cálculos de emissões e cenários continuam sendo feitos pelo próprio site.

## Custos e limites

- O site pode ser publicado no plano gratuito do Cloudflare Pages.
- Workers AI oferece uma cota gratuita diária (a documentação informa 10.000 Neurons/dia no plano Workers Free; limites e modelos podem mudar).
- A cota é compartilhada por todas as pessoas que usarem o assistente. Ao esgotar a cota, a IA pode parar de responder até a renovação diária.
- Para evitar cobrança, permaneça nos planos **Free** e não faça upgrade para plano pago. Não adicione cartão ou serviços pagos se não quiser correr risco de cobrança.
- Como a IA ficará pública, não compartilhe informações confidenciais no chat. Para um projeto maior, configure proteção contra abuso/limites de uso no Cloudflare.

## Estrutura

- `index.html` — o site CarbonPath com a nova aba de IA.
- `functions/api/assistant.js` — endpoint no servidor; a chamada à IA não expõe chaves no navegador.
- `wrangler.toml` — configuração do binding Workers AI (`AI`).

## Publicar sem pagar usando GitHub + Cloudflare Pages

### 1. Crie uma conta e um repositório

1. Crie uma conta gratuita em [GitHub](https://github.com/).
2. Crie um repositório novo, por exemplo `carbonpath-2050`.
3. Envie para esse repositório **todos os arquivos e pastas deste pacote**, mantendo `index.html`, `functions/` e `wrangler.toml` na raiz. Não envie apenas o HTML.

### 2. Publique no Cloudflare Pages

1. Crie uma conta gratuita em [Cloudflare](https://dash.cloudflare.com/).
2. No painel, abra **Workers & Pages** e crie uma aplicação Pages conectada ao GitHub.
3. Selecione o repositório `carbonpath-2050`.
4. Como é um site HTML puro, deixe o comando de build vazio ou use `exit 0`; diretório de saída: `.` (raiz do repositório).
5. Faça o deploy. O Cloudflare fornecerá um endereço público `*.pages.dev`.

### 3. Ative o Workers AI

1. No painel Cloudflare, abra seu projeto em **Workers & Pages**.
2. Em **Settings** (Configurações), procure a seção **Bindings** (Vinculações) do ambiente de produção.
3. Adicione uma vinculação **Workers AI** e use exatamente `AI` como nome/variável. Se o painel indicar que a configuração do `wrangler.toml` já criou a vinculação, apenas confirme.
4. Salve e faça um novo deploy. A documentação atual também descreve essa configuração em [Bindings de Pages Functions](https://developers.cloudflare.com/pages/functions/bindings/).

### 4. Teste

1. Abra o endereço `*.pages.dev`.
2. Clique em **Assistente** e depois em **IA gratuita**.
3. Experimente “Qual fonte de emissão devo priorizar e por quê?”.
4. Se aparecer uma mensagem de configuração, confira o binding `AI` e faça novo deploy. Se aparecer erro de limite/indisponibilidade, tente novamente mais tarde.

## Observações

- O endpoint fica em `/api/assistant` e só aceita `POST` com uma pergunta.
- O modelo configurado é `@cf/meta/llama-3.1-8b-instruct`. A disponibilidade do catálogo pode mudar; se Cloudflare indicar que o modelo não está disponível, escolha um modelo de geração de texto gratuito no catálogo e atualize a constante `MODEL` em `functions/api/assistant.js`.
- A IA pode cometer erros. Os valores do banco de tecnologias são dados de demonstração e precisam de validação técnica antes de qualquer decisão real.
- A integração não exige uma chave de API no HTML. **Nunca coloque tokens secretos no JavaScript do navegador.**
