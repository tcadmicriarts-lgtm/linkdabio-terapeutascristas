# Formação Terapeutas Cristãs — Link da Bio

Página oficial de captura de leads da Formação Terapeutas Cristãs.

## Como funciona o formulário

Quando a pessoa preenche **Nome, E-mail e WhatsApp** e envia:

1. Os dados são enviados via **POST (fetch)** direto para o Google Apps Script:
   `https://script.google.com/macros/s/AKfycbxBASVJ9ZZSr05XFlaLzZGayqZOcg_R_FS2mTswMfVH16M_vHZoBTt3UZKQSDySlznc/exec`

   Payload enviado (JSON):
   ```json
   {
     "nome": "...",
     "email": "...",
     "whatsapp": "...",
     "origem": "Link da Bio - Instagram",
     "data": "2026-07-23T21:45:00.000Z",
     "userAgent": "..."
   }
   ```

2. Se o envio for bem-sucedido, a pessoa é levada à **tela de confirmação com 2 botões**:
   - **Botão 1 — Baixar Presente** → abre o PDF *"Desafio de 30 dias de oração"* no Google Drive (nova aba).
   - **Botão 2 — Entrar no Grupo VIP** → abre o grupo oficial das aulas gratuitas no WhatsApp (nova aba).

3. Se o envio falhar (falta de internet, script fora do ar), o formulário exibe uma mensagem de erro e mantém a pessoa na tela de cadastro para tentar de novo.

Nenhum redirecionamento acontece automaticamente — a pessoa clica em cada botão quando quiser.

## Onde editar os links e a integração

Abra `index.html` e localize o bloco `const CONFIG = { ... }` (por volta da linha 990):

```js
// Link do Presente (PDF no Google Drive)
pdfPresente: "https://drive.google.com/file/d/1Is1hnFk6jnzvNI7p5sx-La_wdQu9ZbKj/view?usp=sharing",

// Link do Grupo VIP (WhatsApp)
linkGrupoAula: "https://chat.whatsapp.com/JAoqm6FRyrj43Bt1ng5RGe",

// URL do Google Apps Script (Web App publicado)
googleSheetsWebhook: "https://script.google.com/macros/s/AKfycbxBASVJ9ZZSr05XFlaLzZGayqZOcg_R_FS2mTswMfVH16M_vHZoBTt3UZKQSDySlznc/exec",
```

Basta trocar as URLs entre aspas e salvar.

## Estrutura

```
formacao-terapeutas-cristas/
├── index.html           ← página principal (formulário + confirmação)
├── painel.html          ← painel interno de configuração
├── assets/
│   ├── logo.png
│   ├── favicon.png
│   ├── og-image.jpg
│   └── presente.pdf     ← backup local (não é mais usado — o link aponta para o Drive)
├── robots.txt
├── sitemap.xml
├── vercel.json
├── COMO-ATIVAR-GOOGLE-SHEETS.md
└── README.md
```

## Publicação

Faça upload de todos os arquivos para a raiz do seu domínio (Vercel, Netlify, hospedagem tradicional, etc.). Não requer build.

## Observação técnica sobre `mode: 'no-cors'`

O Apps Script Web App não retorna cabeçalhos CORS para chamadas anônimas — por isso o `fetch` usa `mode: 'no-cors'`. A promessa resolve assim que a requisição sai do navegador; a gravação na planilha é executada no lado do Google. Se você quiser confirmar cada envio com uma resposta legível no navegador, publique o script com `deploy → new deployment → Web App → Anyone` e adaptando a leitura dos parâmetros no `doPost(e)` para aceitar `e.postData.contents`.
